---
title: Best practices to use Asset Compute Service.
description: Best practices, limitations, and tips to create custom workers using Asset Compute Service.
---

# Best practices and troubleshooting {#best-practices}

**TBD**:

Add any best practices for developers in this section:

* Any items to take care of when creating projects.
* Any naming conventions, reserved keywords, etc.?
* Any terms that can become a source of confusion later based on our OOTB naming.

**TBD**:

Add possible troubleshooting steps to this section:

* APIs are not added to the Adobe I/O project.
* File permissions issues on the local file system.
* Indentation of YAML file is incorrect. Link to the sample YAML file.
* When setting the Journal for the first time, it may fail with a 500 error code. Re-run the Journal setup to complete the setup.

## Minimal Docker image {#minimal-docker-image}

If you have to build a custom docker image you may need to install files that are only needed during the build process.  Be sure that these extra files are not included in the final docker image.  Doing so will cause the image to be larger than necessary and slow down the start up time.

## Single focused {#single-focused}

For custom container workers, they should only wrap a single tool if that's enough for the action(s) using it. Warm containers in OpenWhisk are never reused for another action, so there is no benefit. Images would get too large, maintenance and testing becomes hard due to the unnecessary coupling (e.g. updating to a new version). This is ok however, if the action requires calling both tools in succession for example, and it would be inefficient having to copy the intermediate result (binary) from one worker to the next, via binary cloud storage as indirection.

## CLI app or server {#cli-app-or-server}

The simplest is often to wrap an existing command line app as a Docker container, and a small action that invokes the app and passes the rendition parameters from the request to the corresponding CLI arguments, to generate a desired rendition.

It is also possible to wrap a web server in a container and call it via localhost from the action, either streaming the response into a file or exchange through the common file system (really depends on the server). Just make sure it is not using the port 8080, as that's already reserved for the OpenWhisk runtime server that needs to run on each container.

## Startup time {#startup-time}

A command line app can occur startup cost for every single invocation of the worker (action). Sometimes this can be pretty high, for example if the app needs to initialize libraries or some database. But even if it's a small amount, it affects the overall throughput and the performance of a hot container.

In this case it is useful to have a web server or daemon approach, communicating through HTTP, TCP or sockets with a service that runs in the background and only starts once, when the container starts.

## Support multiple renditions at once {#support-multiple-renditions-at-once}

A worker in Asset Compute Service may create multiple renditions at once, from the same source file. In this case it’s good if the CLI app can do all renditions at once, to not occur the source file load time (etc.) for each rendition.

This might not be possible for an existing CLI tool that cannot be changed easily. But if you provide the CLI tool or can contribute to it, then it makes sense to support CLI arguments that support generating N renditions with different options.

## Security {#security}

After every invocation, a worker (as in hot container) needs to erase any data of the previous invocation. This is because the next invocation can come from a different customer who should under no circumstances be able to get access to other customer's data. The worker library takes care of that for the source file and renditions on disk. The action itself and any CLI app, server or daemon used must ensure it removes any data from memory about the source or rendition files. This is especially required if the container can be used with custom actions that could try and get to the data, even if the standard worker action that might be used for this container isn't allowing such an option.

## Show errors {#show-errors}

If your worker is a Shell script be sure to return the status from the process that does the work.  Frequently, Shell scripts do cleanup at the end and you don't want the status from the shell script be that from those commands.  If your worker is written in JavaScript make sure any errors that may come from cleanup on failure don't generate their own errors that hide the original problem.

## Use custom application in Experience Manager {#use-in-aem}

For information, see [use asset microservices in Experience Manager Assets](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/asset-microservices-overview.html).
