## Worker API/SDK

Extending Nui with new workers is made simple through libraries, APIs and build tooling.

* Current [worker library](https://git.corp.adobe.com/nui/asset-compute-sdk)
* Workers are packaged (including automatically wrapping shell script workers) and tested using
  the [nui cli](https://git.corp.adobe.com/nui/cli)
  * `nui package` package worker/action
  * `nui deploy` deploy worker/action
  * `nui test-worker` unit test runner specific to workers
  * `nui run-worker` run worker locally for development


### Node.js worker

To be specified. For now, see the [javascript code here](https://git.corp.adobe.com/nui/asset-compute-sdk/blob/master/library.js), especially the exports at the bottom. This will change.

To see an example of a simple JavaScript worker which uses our standard docker image see [dcx worker](https://git.corp.adobe.com/nui/worker-dcx/blob/master/worker.js).  This is a worker that also takes advantage of GraphicsMagick which is included as part of our standard docker image.  Workers can use GraphicsMagick both to produce a format that the worker itself may not support and to resize an image.

For an example of a JavaScript worker that makes use of a custom docker image see [Flite worker](https://git.corp.adobe.com/nui/worker-flite/blob/master/action/worker.js).  This worker uses a custom docker image [Flite Dockerfile](https://git.corp.adobe.com/nui/worker-flite/blob/master/docker/Dockerfile) which includes the Adobe Switch Engine to create image renditions.

The most complex worker we currently have is the [PIE (Photoshop Imaging Engine) worker](https://git.corp.adobe.com/nui/worker-pie/blob/master/action/worker.js).  Unlike all other workers we currently have this one creates all of the renditions with one invocation of the program it uses instead of using the library function [forEachRendition](https://git.corp.adobe.com/nui/asset-compute-sdk/blob/master/library.js#L634-L636) to create one rendition at a time.  It also uses a custom docker image [PIE Dockerfile](https://git.corp.adobe.com/nui/worker-pie/blob/master/docker/Dockerfile) to bring in the PIE based renderer. This docker image itself makes use of a special docker image [Compiler Dockerfile](https://git.corp.adobe.com/nui/worker-pie/blob/master/compiler/Dockerfile) to build the executable that is bundled.


### Shell script worker

If your logic to create renditions is simple, and depends upon native code you may wish to write your worker as a simple shell script.  An example with some of the environment variables described can be found here: [Tika worker.sh](https://git.corp.adobe.com/nui/worker-tika/blob/master/action/worker.sh).  The docker image can be found [here](https://git.corp.adobe.com/nui/worker-tika/blob/master/docker/Dockerfile)

### Error reporting

The type of errors that Nui reports to the client can be found [here](https://git.corp.adobe.com/nui/asset-compute-sdk/blob/master/errors.js).  If your worker can tell if it is given a file that it doesn't support it should use `SourceFormatUnsupported` if it a file format that it doesn't support or `SourceUnsupported` if it is a format it supports, but for some reason this particular file is not supported, or `SourceCorruptError` if the source is determined to be corrupt.  An example, would be a worker that handles TIFF files that is given a TIFF file with a particular compression type that the worker does not support.

If the worker is written in JavaScript it can throw the proper error.  If it is a shell script based worker it can produce an error in JSON format and write it to a file named `error.json` in the output directory.  See the [PDF rasterizer worker](https://git.corp.adobe.com/nui/worker-pdfrasterizer/blob/master/action/worker.sh#L14-L24) for an example.

### Testing your worker

For creating unit tests and running them see [nui test-worker](https://git.corp.adobe.com/nui/asset-compute-cli#test-worker). You can use `nui run-worker` to create renditions for your unit tests.  However, if you have a large set of test cases you may wish to use the `-u` option with `nui test-worker`     For testing your worker in the full environment use [Meahana](https://git.corp.adobe.com/nui/meahana).

### Releasing your docker image

If your worker has a special docker image that it uses that image needs to be pushed to the appropriate repository.  This currently is a manual process.  To do this do the following
* Go to [ci-deploy-runtime](https://nui.ci.corp.adobe.com/blue/organizations/jenkins/ci-deploy-runtime/activity) in Jenkins
* Make sure you are logged in
* Click on the `Run` button above the `STATUS` column.
* Click on the circle right below the `STATUS` label.
* Under where it says `Provide Docker runtime to push` there are 3 text boxes
* In the first enter the name of your worker
* In the second enter the directory where the Docker file resides, typically either `runtime` or `docker`
* In the third leave the branch set to `master`
* Click on the `PUSH` button
This will push the runtime to the repo.

## Releasing your action

There is a script named `nui-release.sh` which resides in [infra/scripts](https://git.corp.adobe.com/nui/infra/blob/master/scripts/nui-release.sh).  From your `action` directory first run `nui-release.sh`.  That does a dry run.  If that looks good then run `nui-release.sh do` which will update the version and publish it.

## Best Practices

#### Minimal docker image

If you have to build a custom docker image you may need to install files that are only needed during the build process.  Be sure that these extra files are not included in the final docker image.  Doing so will cause the image to be larger than necessary and slow down the start up time.

#### Single focused

For custom container workers, they should only wrap a single tool if that's enough for the action(s) using it. Warm containers in OpenWhisk are never reused for another action, so there is no benefit. Images would get too large, maintenance and testing becomes hard due to the unnecessary coupling (e.g. updating to a new version). This is ok however, if the action requires calling both tools in succession for example, and it would be inefficient having to copy the intermediate result (binary) from one worker to the next, via binary cloud storage as indirection.

#### CLI app or server

The simplest is often to wrap an existing command line app as a Docker container, and a small action that invokes the app and passes the rendition parameters from the request to the corresponding CLI arguments, to generate a desired rendition.

It is also possible to wrap a web server in a container and call it via localhost from the action, either streaming the response into a file or exchange through the common file system (really depends on the server). Just make sure it is not using the port 8080, as that's already reserved for the OpenWhisk runtime server that needs to run on each container.

#### Startup time

A command line app can occur startup cost for every single invocation of the worker (action). Sometimes this can be pretty high, for example if the app needs to initialize libraries or some database. But even if it's a small amount, it affects the overall throughput and the performance of a hot container.

In this case it is useful to have a web server or daemon approach, communicating through HTTP, TCP or sockets with a service that runs in the background and only starts once, when the container starts.

#### Support multiple renditions at once

A worker in nui might get to do multiple renditions at once, from the same source file. In this case it’s good if the CLI app can do all renditions at once, to not occur the source file load time (etc.) for each rendition.

This might not be possible for an existing CLI tool that cannot be changed easily. But if you provide the CLI tool or can contribute to it, then it makes sense to support CLI args that support generating N renditions with different options.

#### Security

After every invocation, a worker (as in hot container) needs to erase any data of the previous invocation. This is because the next invocation can come from a different customer who should under no circumstances be able to get access to other customer's data. The worker library takes care of that for the source file and renditions on disk. The action itself and any CLI app, server or daemon used must ensure it removes any data from memory about the source or rendition files. This is especially required if the container can be used with custom actions that could try and get to the data, even if the standard worker action that might be used for this container isn't allowing such an option.

#### Don't hide errors

If your worker is a shell script be sure to return the status from the process that does the work.  Frequently, shell scripts do cleanup at the end and you don't want the status from the shell script be that from those commands.  If your worker is written in Javascript make sure any errors that may come from cleanup on failure don't generate their own errors that hide the original problem.
