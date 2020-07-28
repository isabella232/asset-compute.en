---
title: Set the development environment required for Asset Compute Service.
description: Developer environment setup for Asset Compute Service to start creating and testing custom code.
---

# Setup a developer environment {#create-dev-environment}

To create a setup that allows you to develop for Compute Service, follow these requirements and instructions.

<!-- Attention: Add public-facing multiple links.
-->

1. [Acquire access and credentials](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/setup.md#acquire-access-and-credentials) for Project Firefly.

1. [Set up the local environment](https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/setup.md#local-environment-set-up) and the required tools.

1. Some more tools that help you get started developing smoothly are:

   * [Git](https://git-scm.com/).
   * [Docker](https://www.docker.com) on Mac. Use [Docker for Mac](https://docs.docker.com/docker-for-mac/install/).
   * [Docker Compose](https://docs.docker.com/compose/) (part of Docker for Mac, used for docker builds) (`docker-compose`).
   * [NodeJS](https://nodejs.org) and [NPM](https://www.npmjs.com). User of OSX HomeBrew can do `brew install node` to install both. Otherwise, download it from the [NodeJS download page](https://nodejs.org/en/). Use version: `10.x` or later for the `nui` CLI.
   * An IDE that is good for NodeJS, such as [VS Code](https://code.visualstudio.com) or [Webstorm](https://www.jetbrains.com/webstorm/).
   * [AIO CLI](https://github.com/adobe/aio-cli) (`aio`) - install using `npm install -g @adobe/aio-cli`.
   * [OpenWhisk CLI](https://github.com/apache/incubator-openwhisk-cli) (`wsk`). [Download here](https://github.com/apache/incubator-openwhisk-cli/releases) or `brew install wsk`.
   * [Adobe Asset Compute devtool](https://github.com/adobe/asset-compute-devtool), our web-based developer user interface for Compute Service. To use Adobe Asset Compute devtool, S3 is required. Including access to S3 buckets.
   * (Optional) [ExifTool](https://www.sno.phy.queensu.ca/~phil/exiftool/). Required to validate test results in `worker-cameraraw`. [Download here](https://www.sno.phy.queensu.ca/~phil/exiftool/install.html) or `brew install exiftool`.
   * (Optional) [I/O Runtime Shell UI](https://git.corp.adobe.com/cloudshell/experimental-shell). [Download here](https://git.corp.adobe.com/cloudshell/experimental-shell/releases), OSX only right now.

1. Ensure to meet the [prerequisites](release-notes.md#prerequisites).

**TBD**:

Update this section with the following information:

* Any steps in the beginning that lead to gotchas later should be called out for caution? For example,
  * don't change some defaults initially
  * know risks when deviating from standard path
  * naming conventions to follow
  * Retrieve and format credentials (YAML file details)

## Test your setup {#test-setup}

You can test the setup by testing a custom worker. See https://git.corp.adobe.com/nui/example-custom-worker

**TBD**:

Bring over that information to this article.
