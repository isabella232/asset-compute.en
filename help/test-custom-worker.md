---
title: Test and debug Asset Compute Service custom worker.
description: Test and debug Asset Compute Service custom worker.
---

# Test a custom application {#test-custom-worker}

To test a custom worker, run the following command in the root of the application:

```
aio app test
```

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

The mock file must be a JSON formatted http response using the documentation [here](https://www.mock-server.com/mock_server/creating_expectations.html). If there are multiple host names to mock, define multiple mock-*.json files. 
Here is an exxample mock file for `google.com` named `mock-google.com.json`:

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
