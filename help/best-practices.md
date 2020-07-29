---
title: Best practices to use Asset Compute Service.
description: Best practices, limitations, and tips to create custom workers using Asset Compute Service.
---

**TBD**

- limitations for custom workers
- best practices => future (do NOT copy any content from https://git.corp.adobe.com/nui/nui/blob/master/doc/worker_api.md which is out of date and mostly not relevant for 3rd party custom workers)

# Troubleshooting {#troubleshooting}

* If you had issues logging in to the Adobe Developer Console [through the Adobe I/O CLI](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#3-signing-in-from-cli), you will need to manually add some credentials needed for developing, testing, and deploying your custom worker:
    1. Navigate to your Firefly Project and Workspace on the [Adobe Developer Console](https://console.adobe.io/) and and press `"Download"` in the top right corner. Open this file and save this JSON to a safe place on your machine.
    2. Navigate to the `.env` file in your Firefly Application
    3. Add the Adobe I/O Runtime Credentials
        - Get the AIO Runtime credentials from the downloaded JSON. They will be under `project.workspace.services.runtime`.
        - Add the I/O Runtime credentials in the `AIO_runtime_XXX` variables:
        ```
        AIO_runtime_auth=
        AIO_runtime_namespace=
        ```
    4. Add the absolute path to the downloaded JSON in Step 1:
        ```
        ASSET_COMPUTE_INTEGRATION_FILE_PATH=
        ```
    Make sure to set up the rest of the [necessary credentials](.develop-custom-worker.md#developer-tool-credentials) needed for the developer tool.
    
* When running the developer tool for the first time with a new Asset Compute Integration, it may fail the first processing request because the Asset Compute Events Journal is not fully set up. Wait some time for the journal to set up before sending another request.
* If you run into errors sending Asset Compute `/register` or `/process` requests, check to make sure all the necessary APIs are added to the Adobe I/O Project and Workspace:
    -  `"Asset Compute"`, `"IO Events"`, `"IO Events Management"`, and `"Runtime"`

## General troubleshooting tips

- Make sure your Javascript worker doesn't crash on startup. If this happens, it is usually related to a missing library or dependency.
- Make sure that all dependencies that need installation are referenced in the worker's `package.json` file.
- Make sure any errors that may come from cleanup on failure don't generate their own errors that hide the original problem.

**TBD**:

Add any best practices for developers in this section:

* Any items to take care of when creating projects.
* Any naming conventions, reserved keywords, etc.?
* Any terms that can become a source of confusion later based on our OOTB naming.

**TBD**:

Add possible troubleshooting steps to this section:

* File permissions issues on the local file system.

## Use custom application in Experience Manager {#use-in-aem}

For information, see [use asset microservices in Experience Manager Assets](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/asset-microservices-overview.html).
