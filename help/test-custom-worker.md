---
title: Test and debug Asset Compute Service custom worker.
description: Test and debug Asset Compute Service custom worker.
---

# Test a custom application {#test-custom-worker}

Make sure to have [Docker Desktop](https://www.docker.com/get-started) installing and running on your machine.

To test a custom worker, run the following command in the root of the application:

```
aio app test
```

# Debug a custom application {#debug-custom-worker}

The following steps show how you can debug your custom worker using Visual Studio Code. It allows for seeing live logs, hit breakpoints and step through code as well as live reloading of local code changes upon every activation.

_Please note that many of these steps are usually automated by `aio` out of the box, see section "Debugging the Application" in the [Firefly documentation](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html#!AdobeDocs/project-firefly/master/getting_started/first_app.md). Due to some issues below steps include a workaround for some temporary issues that will be addressed soon._

1. install latest [wskdebug](https://github.com/apache/openwhisk-wskdebug) from github, plus the optional [ngrok](https://www.npmjs.com/package/ngrok)

   ```
   npm install -g @openwhisk/wskdebug
   npm install -g ngrok --unsafe-perm=true
   ```

2. add this to your User settings.json (keeps using the old VS Code debugger, the new one has [some issues](https://github.com/apache/openwhisk-wskdebug/issues/74) with wskdebug):
   ```
   "debug.javascript.usePreview": false
   ```

3. make sure you have NO `aio app run` running

4. deploy latest code

   ```
   aio app deploy
   ```
  
5. run just the Asset compute devtool (and keep it running)

   ```
   npx adobe-asset-compute devtool
   ```

6. in VS Code, add this debug/run configuration to your launch.json:

    ```
    {
      "type": "node",
      "request": "launch",
      "name": "wskdebug worker",
      "runtimeExecutable": "wskdebug",
      "args": [
        "PROJECT-0.0.1/__secured_worker",           // <--- replace this with your ACTION NAME
        "${workspaceFolder}/actions/worker/index.js",
        "-l",
        "--ngrok"
      ],
      "localRoot": "${workspaceFolder}",
      "remoteRoot": "/code",
      "outputCapture": "std",
      "timeout": 30000
    }
    ```
  
   To get the ACTION NAME, look at the output of `aio app deploy`:
  
    ```
    Your deployed actions:
      -> TypicalCoffeeCat-0.0.1/__secured_worker 
    ```
7. select `wskdebug worker` from the run/debug configuration and hit run (play icon)
8. wait for it to start, should say "🚀 Ready for activations" in the "Debug Console" window at the bottom when ready
9. click run in the devtool
10. after a moment, you should see the action executing in VS Code, see the logs appearing
11. set a breakpoint in your code, run again and it should hit
12. if you make code changes, they are hot reloaded and effective as soon as the next activation happens

Note: You will always see 2 activations for each request in custom workers. This is because the first is a web action that will simply invoke itself asynchronously (this all happens in the SDK code). The second activation is then the one that will hit your code.

## Adding Worker Tests

### Adding tests
Tests are expected inside the `test` folder at the root level of the aio project. The test cases for each worker should be in the path `test/asset-compute/<worker-name>`, with one folder for each test case:

```
action/
manifest.yml
package.json
...
test/
  asset-compute/
    <worker-name>/
        <testcase1>/
            file.jpg
            params.json
            rendition.png
        <testcase2>/
            file.jpg
            params.json
            rendition.gif
            validate
        <testcase3>/
            file.jpg
            params.json
            rendition.png
            mock-adobe.com.json
            mock-console.adobe.io.json
```

Have a look at [example custom workers](https://github.com/adobe/asset-compute-example-workers/) for some examples. Below is a detailed reference.

### Test output
The detailed test output including the logs of the custom worker will be put in the `build` folder at the root of the Firefly app.

### Mocking external services
It is possible to mock external service calls in your actions by defining `mock-<HOST_NAME>.json` files in your test cases, where HOST_NAME is the host you would like to mock. An example use case is a worker that makes a separate call to S3. The new test structure would look like this:

```
      <testcase3>/
        file.jpg
        params.json
        rendition.png
        mock-<HOST_NAME1>.json
        mock-<HOST_NAME2>.json
```

The mock file must be a JSON formatted http response using the documentation [here](https://www.mock-server.com/mock_server/creating_expectations.html). If there are multiple host names to mock, define multiple mock-<mocked-host>.json files. 
Here is an exaample mock file for `google.com` named `mock-google.com.json`:

```
[{
    "httpRequest": {
		"path": "/images/hello.txt"
		"method": "GET"
    },
    "httpResponse": {
        "statusCode": 200,
        "body": {
          "message": "hello world!"
        }
    }
}]
```

#### Sharing files across test cases

It is recommended to use relative **symlinks** if you share `file.*`, `params.json` or `validate` scripts across multiple tests. They are supported with git. Make sure to give your shared files a unique name, since you might have different ones. In the example below the tests are mixing and matching a few shared files, and their own:

```
tests/
    file-one.jpg
    params-resize.json
    params-crop.json
    validate-image-compare
    
    jpg-png-resize/
        file.jpg    - symlink: ../file-one.jpg
        params.json - symlink: ../params-resize.json
        rendition.png
        validate    - symlink: ../validate-image-compare

    jpg2-png-crop/
        file.jpg
        params.json - symlink: ../params-crop.json
        rendition.gif
        validate    - symlink: ../validate-image-compare

    jpg-gif-crop/
        file.jpg    - symlink: ../file-one.jpg
        params.json - symlink: ../params-crop.json
        rendition.gif
        validate
```

### Examples
See the [worker-animal-pictures](https://github.com/adobe/asset-compute-example-workers/tree/master/projects/worker-animal-pictures/test/asset-compute/worker-animal-pictures) for an example.

## Error reporting {#error-reporting}

The type of errors that Asset Compute Service reports to the client are available [here](https://github.com/adobe/asset-compute-commons/blob/master/lib/errors.js). For a custom worker to report an unsupported file, use `SourceFormatUnsupported`. if it a file format that it doesn't support or `SourceUnsupported` if it is a format it supports, but for some reason this particular file is not supported, or `SourceCorruptError` if the source is determined to be corrupt.  An example, would be a worker that handles TIFF files that is given a TIFF file with a particular compression type that the worker does not support.

### Testing expected errors

Error tests cases should not contain an expected `rendition.*` file and should define the expected `errorReason` inside the `params.json` file.

#### Error Test Case Structure
```
<error_test_case>/
    file.jpg
    params.json
```

#### Parameter file with error reason
```js
{
    "errorReason": "SourceUnsupported",
    // ... other params
}
```

See full list and description of Asset Compute error reasons [here](https://github.com/adobe/asset-compute-commons#error-reasons)
