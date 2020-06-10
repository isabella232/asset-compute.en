---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that helps reduce complexity and improves scalability.
---

# Overview of Asset Compute Service {#overview}

Project Firefly is a framework to build and deploy custom web applications on Adobe I/O Runtime. Developers can leverage React Spectrum (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs to create custom apps for business users.

Asset Compute is a scalable, lightweight, and extensible platform service to process assets for AEM. The service works completely on the Adobe I/O runtime. It is easily extendable by creating custom workers based off Project Firefly. These custom workers are Project Firefly apps and can be written to do tasks such as add custom conversion tools, or call external APIs to perform image operations.

See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

<!-- 
* Introduction/ PM Overview.
* Some info about benefits is at https://git.corp.adobe.com/nui/nui/blob/master/whitepaper/Project-Nui-Asset-Compute-Service.md#benefits-for-aem-assets
-->

## Get to know Compute Service {#get-to-know}

<!-- 
* Explain extensibility in detail.
* Supported use cases.
* Benefits of this method over previous method.
* Key overview section for developers.
-->

### What Compute Service can do for you {#example-of-use-cases}

<!-- Explain a few use cases as one-liners to illustrate the needs that Asset Compute Service can fulfill. -->

### Prerequisites {#prerequisites}

To create and deploy and custom worker, fulfil the following requirements:

* Be a part of an Adobe Experience Cloud organization. <!-- how to check and where to ask? -->
* Ensure that Adobe Experience Manager as a Cloud Service is enabled for the Adobe Experience Cloud organization. <!-- how to check and where to ask? -->
* Ensure that the Experience Cloud organization is part of the [!DNL Project Firefly] developer preview program. See [how to apply for access](https://github.com/AdobeDocs/project-firefly/blob/master/overview/getting_access.md).
* Developer role or administrator permissions in the organization.
* Ensure that Adobe I/O CLI is installed locally. <!-- Link to CLI GitHub for more info. -->

<!-- Also, see https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#technical-requirements
-->