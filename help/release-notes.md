---
title: Release notes of Asset Compute Service.
description: New features, enhancements, and known issues of Asset Compute Service.
---

# Release notes of Asset Compute Service {#release-notes}

The latest release of Asset Compute Service is released on Jul 30, 2020.

**TBD:**

* Are we versioning the releases?
* Is there any compatibility information to be added? With Project Firefly versions, or AEMaaCS releases, or other offerings/integrations such as InDesign Server?

## Prerequisites {#prerequisites}

To create and deploy and custom worker, fulfill the following requirements:

* Ensure that the [!DNL Adobe Experience Cloud] organization is part of the [!DNL Project Firefly] developer preview program. See [how to apply for access](https://github.com/AdobeDocs/project-firefly/blob/master/overview/getting_access.md).
* Ensure that the [!DNL Adobe Experience Cloud] organization has AEM as a Cloud Service enabled
* Ensure a developer role or administrator permissions in the organization.
* Ensure that [Adobe I/O CLI](https://github.com/adobe/aio-cli) is installed locally.

For testing your custom worker with the [developer tool](https://github.com/adobe/asset-compute-devtool), the following is also required:
- Access to a [cloud storage container](https://github.com/adobe/asset-compute-devtool#prerequisites). Currently, we support Azure Blob Storage and AWS S3.

    _Cloud storage access is only required for using the developer tool. You can still create, test and deploy custom workers with out using the developer tool._

After checking all the prerequisites, you are ready to [set up your environment](setup-environment.md).

## Known issues {#known-limitations}

- Access to a [cloud storage container](https://github.com/adobe/asset-compute-devtool#prerequisites) is **required** for using the [developer tool](https://github.com/adobe/asset-compute-devtool).
    - The cloud storage access is only needed for the developer tool. You can still create, test, and deploy custom workers without developer tool.
    - This can be a shared container used by mutliple developers across different projects.
    - This cloud storage is different than the AEM blob store.

**TBD:**

This section can be updated with the following information:

* Also, see https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#technical-requirements
* Anything more? Separate storage, AIO access, what technical know-how/skills are required/assumed for the developer persona.

## Contribute to open-source Asset Compute Service project {#contribute-open-source}

Asset Compute Service is developed under an open development model that welcomes contributions from everyone who is interested in asset and rendition processing. See [how and where to contribute to Compute Service](contribute-to-compute-service.md).
