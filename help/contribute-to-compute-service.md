---
title: Release notes of Asset Compute Service.
description: New features, enhancements, and known issues of Asset Compute Service.
---

# Contribute to open-source Asset Compute Service project {#contribute-to-compute-service-project}

Asset Compute Service is developed under an open development model that welcomes contributions from everyone who is interested in asset and rendition processing. A few ways in which you can contribute are:

* Share feedback: If you see a product or documentation issue or have a new feature idea or improvement request, create a GitHub issue in the relevant repository. 

  _If you are not sure which repository to submit your issue, log an issue in [Asset Compute SDK repository](https://github.com/adobe/asset-compute-sdk)._
* Patch: To provide a patch, fork and provide a pull request.

Members and [code owners](https://help.github.com/articles/about-codeowners/) review the pull requests and accept or reject the changes. You can not make direct changes to the `main` (aka `master`) branch, as you will need to create a pull request beforehand to be reviewed and merged into the main branch.

## Contribution guidelines {#contribution-guidelines}

Asset Compute Service has repository modularity and naming guidelines. It is modular to the extent possible, as fostered by the serverless concept and OpenWhisk framework. It means having small and focused GitHub repositories that support decoupled development and deployment lifecycles. One repository for one action is OK if it represents its own small service such as a worker. If you want to create a separate repository, log an issue in [Asset Compute SDK repository](https://github.com/adobe/asset-compute-sdk).

For detailed guidelines, see the [contribution guidelines](https://github.com/adobe/asset-compute-sdk/blob/master/.github/CONTRIBUTING.md). Also, follow these [Git commit message guidelines](https://chris.beams.io/posts/git-commit/).

## Available resources and libraries {#available-resources}

[Here](https://github.com/adobe/asset-compute-sdk#available-resources-and-libraries) is a full list of Asset Compute Service open-sourced libraries.

The available Adobe I/O Runtime and OpenWhisk resources are:

* [Get started with Apache OpenWhisk](https://github.com/apache/incubator-openwhisk/tree/master/docs#getting-started-with-openwhisk).
* [Lab: Build composable AI with Adobe Sensei functions and Adobe I/O Runtime](https://opensource.adobe.com/adobe-sensei-ai-functions/index.html/).
* [Adobe I/O Runtime FAQ](https://www.adobe.io/apis/experienceplatform/runtime/docs.html#!adobedocs/adobeio-runtime/master/resources/faq.md)
