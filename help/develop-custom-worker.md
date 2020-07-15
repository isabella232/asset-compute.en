---
title: Develop for Asset Compute Service.
description: Create customization using Asset Compute Service.
---

# Develop a custom applications {#develop}

**TBD**:

Seed content available at:

* https://git.corp.adobe.com/nui/nui/blob/master/dev/develop.md
* https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md

## Tools required {#tools-required}

## Try the sample worker provided by Adobe {#try-sample}

**TBD**:

* See https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#example-custom-workers

### Node.js worker

<!-- TBD: Add correct path here if required. Else remove the first paragraph.
-->

To be specified. For now, see the [JavaScript code here](https://github.com/adobe/asset-compute-sdk), especially the exports at the bottom. This will change.

To see an example of a simple JavaScript worker which uses our standard docker image see [dcx worker](https://git.corp.adobe.com/nui/worker-dcx/blob/master/worker.js).  This is a worker that also takes advantage of GraphicsMagick which is included as part of our standard docker image.  Workers can use GraphicsMagick both to produce a format that the worker itself may not support and to resize an image.

For an example of a JavaScript worker that makes use of a custom docker image see [Flite worker](https://git.corp.adobe.com/nui/worker-flite/blob/master/action/worker.js).  This worker uses a custom docker image [Flite Dockerfile](https://git.corp.adobe.com/nui/worker-flite/blob/master/docker/Dockerfile) which includes the Adobe Switch Engine to create image renditions.

The most complex worker we currently have is the [PIE (Photoshop Imaging Engine) worker](https://git.corp.adobe.com/nui/worker-pie/blob/master/action/worker.js).  Unlike all other workers we currently have this one creates all of the renditions with one invocation of the program it uses instead of using the library function [forEachRendition](https://git.corp.adobe.com/nui/asset-compute-sdk/blob/master/library.js#L634-L636) to create one rendition at a time.  It also uses a custom docker image [PIE Dockerfile](https://git.corp.adobe.com/nui/worker-pie/blob/master/docker/Dockerfile) to bring in the PIE based renderer. This docker image itself makes use of a special docker image [Compiler Dockerfile](https://git.corp.adobe.com/nui/worker-pie/blob/master/compiler/Dockerfile) to build the executable that is bundled.

### Shell script worker

If your logic to create renditions is simple, and depends upon native code you can create your worker as a simple Shell script. An example is [Tika worker.sh](https://git.corp.adobe.com/nui/worker-tika/blob/master/action/worker.sh) script with some of the environment variables described. The Docker image is available [here](https://git.corp.adobe.com/nui/worker-tika/blob/master/docker/Dockerfile).

## Create a custom worker {#create-custom-worker}

See https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#4-bootstrapping-new-app-using-the-cli

## Execute the worker app {#run-custom-worker}

**TBD**:

* See https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#running-the-application
