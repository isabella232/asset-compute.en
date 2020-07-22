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

To add additional worker tests, follow the guidlines.
 <!-- either add this information here or link it: https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/AddWorkerTests.md -->

<!-- example worker tests: https://github.com/adobe/asset-compute-example-workers/tree/master/projects/worker-animal-pictures/test/asset-compute/worker-animal-pictures -->

## Error reporting {#error-reporting}

The type of errors that Asset Compute Service reports to the client are available [here](https://github.com/adobe/asset-compute-commons/blob/master/lib/errors.js). For a custom worker to report an unsupported file, use `SourceFormatUnsupported`. if it a file format that it doesn't support or `SourceUnsupported` if it is a format it supports, but for some reason this particular file is not supported, or `SourceCorruptError` if the source is determined to be corrupt.  An example, would be a worker that handles TIFF files that is given a TIFF file with a particular compression type that the worker does not support.

If the worker is written in JavaScript it can throw the proper error. If it is a Shell script based worker it can produce an error in JSON format and write it to a file named `error.json` in the output directory. 

<!-- See the [PDF Rasterizer worker](https://git.corp.adobe.com/nui/worker-pdfrasterizer/blob/master/action/worker.sh#L14-L24) for an example. 

TBD: Possible to provide a publicly-available example from an Adobe-provided example of a custom worker? -->
