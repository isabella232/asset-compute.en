---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that reduces complexity and improves scalability.
---

# Overview of Asset Compute Service {#overview}

Asset Compute Service is a scalable, lightweight, and extensible platform service to process assets. Asset Compute Service provides scalable and resilient processing of assets using cloud services. Adobe manages the cloud services for optimal handling of different asset types and processing options.

Also, it provides design and framework for a developer to create a custom asset worker that extends the default asset processing service. Asset Compute Service provides [!DNL Adobe Sensei] support for digital assets. The service works on the [!DNL Adobe I/O] runtime. It is extendable by creating custom workers based on [!DNL Project Firefly]. These custom workers are [!DNL Project Firefly] apps and do tasks such as add custom conversion tools or call external APIs to perform image operations.

[!DNL Project Firefly] is a framework to build and deploy custom web applications on [!DNL Adobe I/O] runtime. To create custom applications, the developers can leverage [!DNL React Spectrum] (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs. See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

---
TBD:

* Add an introduction, PM overview.

## What Compute Service can do for you {#possible-use-cases-benefits}

The Asset Compute Service supports standard use cases like basic image processing; Adobe application specific conversions; and custom workers creation that orchestrate complex business requirements.

You can use [!DNL Asset Compute] web service to generate thumbnails for different file types, high-quality image renderings for the [supported file formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html), extract metadata, extract full text as precursor for indexing, and use the supported [!DNL Adobe Sensei] services on an asset.

You can easily add custom conversion tools, say create composition workflows that combine [!DNL Adobe Sensei] results with image operations.

The service does not do the following on its own, by default.

|Asset Compute Service does not do this|Expectations from implementing client|
|---|---|
| Binary uploads or API-based asset ingestion. | Use other methods to ingest assets. |
| Store binaries or any persisted data across processing requests.| Each request is independent so treat it as a standalone request by sharing binary and processing instructions. |
| Store any configurations such as processing rules or settings for a user or an organization's account. | Add processing request to each request/instruction. |
| Direct event handling of asset creation events from storage systems and processing completed notifications, and errors. | Use Adobe I/O Events and other methods. |

---
TBD:

* Some info about the benefits is at https://git.corp.adobe.com/nui/nui/blob/master/whitepaper/Project-Nui-Asset-Compute-Service.md#benefits-for-aem-assets. See what's worth including.
* Explain extensibility and top-level functional use cases.
* Explain a few specific functional use cases as one-liners to illustrate the needs that Asset Compute Service can fulfill.

## Prerequisites {#prerequisites}

To create and deploy and custom worker, fulfil the following requirements:

* Be a part of an [!DNL Adobe Experience Cloud] organization. <!-- how to check and where to ask? -->
* Ensure that [!DNL Adobe Experience Manager as a Cloud Service] is enabled for the [!DNL Adobe Experience Cloud] organization. <!-- how to check and where to ask? -->
* Ensure that the [!DNL Adobe Experience Cloud] organization is part of the [!DNL Project Firefly] developer preview program. See [how to apply for access](https://github.com/AdobeDocs/project-firefly/blob/master/overview/getting_access.md).
* Ensure a developer role or administrator permissions in the organization.
* Ensure that [!DNL Adobe I/O CLI] is installed locally. <!-- Link to CLI GitHub for more info. -->

---
TBD:

* Also, see https://git.corp.adobe.com/nui/nui/blob/master/doc/developer/CustomWorkerDeveloperGuide.md#technical-requirements
* Anything more? Separate storage, AIO access, what technical know-how/skills are required/assumed for the developer persona.

<!--

TBD: When RNs will be required, uncomment this section.

## Release notes of Asset Compute Service {#release-notes}

-->

>[!MORELIKETHIS]
>
>* [Overview of asset processing with asset microservices in Adobe Experience Manager as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/asset-microservices-overview.html).
>* [Documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).
>* [File formats supported for processing](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html).
