---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that reduces complexity and improves scalability.
---

# Overview of Asset Compute Service {#overview}

Asset Compute Service is a scalable, lightweight, and extensible platform service to process assets. The Asset Compute Service provides Sensei content services support for assets.

The service works completely on the [!DNL Adobe I/O] runtime. It is easily extendable by creating custom workers based off [!DNL Project Firefly]. These custom workers are [!DNL Project Firefly] apps and do tasks such as add custom conversion tools or call external APIs to perform image operations.

Project Firefly is a framework to build and deploy custom web applications on Adobe I/O Runtime. Developers can leverage [!DNL React Spectrum] (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs to create custom apps. See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

<!-- 
* Introduction/ PM Overview.
* Some info about benefits is at https://git.corp.adobe.com/nui/nui/blob/master/whitepaper/Project-Nui-Asset-Compute-Service.md#benefits-for-aem-assets
-->

## What Compute Service can do for you {#example-of-use-cases}

<!-- 
* Explain extensibility in detail.
* Supported top-level use cases.
* Benefits of this method over previous method.
* Explain a few specific use cases as one-liners to illustrate the needs that Asset Compute Service can fulfill. 
-->

You can use Asset Compute web service to generate thumbnails for different file types, high-quality image renderings for the [supported file formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html), extract metadata, extract full text as precursor for indexing, and use the supported [!DNL Adobe Sensei] services on an asset.

You can easily add custom conversion tools, say create composition workflows that combine [!DNL Adobe Sensei] results with image operations.

## Prerequisites {#prerequisites}

To create and deploy and custom worker, fulfil the following requirements:

* Be a part of an Adobe Experience Cloud organization. <!-- how to check and where to ask? -->
* Ensure that Adobe Experience Manager as a Cloud Service is enabled for the Adobe Experience Cloud organization. <!-- how to check and where to ask? -->
* Ensure that the Experience Cloud organization is part of the [!DNL Project Firefly] developer preview program. See [how to apply for access](https://github.com/AdobeDocs/project-firefly/blob/master/overview/getting_access.md).
* Developer role or administrator permissions in the organization.
* Ensure that Adobe I/O CLI is installed locally. <!-- Link to CLI GitHub for more info. -->

<!-- Also, see https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#technical-requirements
-->
