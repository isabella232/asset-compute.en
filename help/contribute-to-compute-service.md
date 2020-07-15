---
title: Release notes of Asset Compute Service.
description: New features, enhancements, and known issues of Asset Compute Service.
---

# Contribute to open-source Asset Compute Service project {#contribute-to-compute-service-project}

Asset Compute Service is developed under an open development model that welcomes contributions from everyone who is interested in asset and rendition processing. A few ways in which you can contribute are:

* Share feedback: If you see a product or documentation issue or have a new feature idea or improvement request, create a GitHub issue in the relevant repository.
* Contribute a new container or a new worker: To contribute a new container or a standard worker, request a new repository by creating an issue in the [Asset Compute Service GitHub repository](https://git.corp.adobe.com/nui/nui/issues/new).
* Patch: To provide a patch, fork and provide a pull request.

Members and [code owners](https://help.github.com/articles/about-codeowners/) review the pull requests and accept or reject the changes. Do not make direct changes to the master.

## Contribution guidelines {#contribution-guidelines}

Asset Compute Service has repository modularity and naming guidelines. It is modular to the extent possible, as fostered by the serverless concept and OpenWhisk framework. It means having small and focused GitHub repositories that support decoupled development and deployment lifecycles. One repository for one action is OK if it represents its own small services such as a worker. If you want to create a separate repository, log an issue in [Asset Compute SDK repository](https://github.com/adobe/asset-compute-sdk).

For detailed guidelines, see the [contribution guidelines](https://github.com/adobe/asset-compute-sdk/blob/master/.github/CONTRIBUTING.md). Also, follow these [Git commit message guidelines](https://chris.beams.io/posts/git-commit/).

## Available resources and libraries {#available-resources}

The open-sourced libraries of Asset Compute Service are:

* [Asset Compute SDK](https://github.com/adobe/asset-compute-sdk): the worker SDK and main framework for third-party custom workers.
* [Asset Compute Commons](https://github.com/adobe/asset-compute-commons): Common utilities needed by all Asset Compute serverless actions.
* [Asset Compute Client](https://github.com/adobe/asset-compute-client): JavaScript client for the Adobe Asset Compute Service.
* [Asset Compute example workers](https://github.com/adobe/asset-compute-example-workers): Samples of third-party Asset Compute worker.
* Adobe I/O Events library.
* [ESlint configuration](https://github.com/adobe/eslint-config-asset-compute): Shared ESLint configuration for Nodejs projects related to the Adobe Asset Compute service.
* [Asset Compute Development Tool](https://github.com/adobe/asset-compute-devtool): Library for the developer tool to explore and to test the Adobe Asset Compute Service.
* [aio-cli-plugin-asset-compute](https://github.com/adobe/aio-cli-plugin-asset-compute): Asset Compute plug-in for Adobe I/O Command Line Interface.
* [Adobe Asset Compute integration tests](https://github.com/adobe/asset-compute-integration-tests): Integration tests for the Asset Compute developer experience.

<!-- Attention: Should we mention the node libraries in docs? 
-->

A few general purpose node libraries are:

* [node-fetch-retry](https://github.com/adobe/node-fetch-retry): Node module to perform retries for HTTP requests.
* [node-httptransfer](https://github.com/adobe/node-httptransfer): Transfer file content from HTTP(S) URLs to HTTP(S) URLs and between HTTP(S) URL.
* [CGROUP-METRICS](https://github.com/adobe/node-cgroup-metrics): Node module for reading `cgroup` metrics. Reads from `/sys/fs/cgroup/`.
* [node-openwhisk-newrelic](https://github.com/adobe/node-openwhisk-newrelic): Gather metrics from Apache OpenWhisk actions and send those to New Relic Insights.
* [node-metrics-sampler](https://github.com/adobe/node-metrics-sampler): Library for collecting and gathering metrics.
* [Cloud Blob Store Wrapper](https://github.com/adobe/node-cloud-blobstore-wrapper): General cloud storage library for Azure and AWS.

The available Adobe I/O Runtime and OpenWhisk resources are:

* [Get started with Apache OpenWhisk](https://github.com/apache/incubator-openwhisk/tree/master/docs#getting-started-with-openwhisk).
* [Lab: Build composable AI with Adobe Sensei functions and Adobe I/O Runtime](https://opensource.adobe.com/adobe-sensei-ai-functions/index.html/).
* [Adobe I/O Runtime FAQ](https://wiki.corp.adobe.com/pages/viewpage.action?pageId=1492581057)
* [OpenWhisk tips and tricks](https://wiki.corp.adobe.com/display/~aklimets/OpenWhisk+Tips+and+Tricks): For example, how to run OW locally.

<!-- attention: Is any public-facing content available on GitHub.com for the above Adobe-internal wiki links?
-->

**TBD**:

* Replace internal links with public-facing links. Remove internal-only content.
