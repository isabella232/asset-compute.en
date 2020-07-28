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

## Single focused {#single-focused}

Workers should only do a single action. Warm containers in OpenWhisk are never reused for another action, so there is no benefit of having workers doing multiple actions. 

## Startup time {#startup-time}

Costs for worker startup can be pretty high, for example if the app needs to initialize libraries or some database. But even if it's a small amount, it affects the overall throughput and the performance of a hot container.

In this case it is useful to have a web server or daemon approach, communicating through HTTP, TCP or sockets with a service that runs in the background and only starts once, when the container starts.

## Security {#security}

After every invocation, a worker (as in hot container) needs to erase any data of the previous invocation. This is because the next invocation can come from a different customer who should under no circumstances be able to get access to other customer's data. The worker library takes care of that for the source file and renditions on disk. The action itself and any CLI app, server or daemon used must ensure it removes any data from memory about the source or rendition files. This is especially required if the container can be used with custom actions that could try and get to the data, even if the standard worker action that might be used for this container isn't allowing such an option.

## Show errors {#show-errors}

- Make sure your Javascript worker doesn't crash on startup. If this happens, it is usually related to a missing library or dependency.
- Make sure that all dependencies that need installation are referenced in the worker's `package.json` file.
- Make sure any errors that may come from cleanup on failure don't generate their own errors that hide the original problem.

## Use custom application in Experience Manager {#use-in-aem}

For information, see [use asset microservices in Experience Manager Assets](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/asset-microservices-overview.html).
