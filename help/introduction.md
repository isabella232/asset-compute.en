---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that reduces complexity and improves scalability.
---

# Overview of Asset Compute Service {#overview}

Asset Compute Service is a scalable, lightweight, and extensible platform service to process assets. Asset Compute Service provides scalable and resilient processing of assets using cloud services. Adobe manages the cloud services for optimal handling of different asset types and processing options.

Also, it provides design and framework for a developer to create a custom asset compute worker that extends the default asset processing service. Extending Asset Compute Service provides a way to easily integration [!DNL Adobe Sensei] services for digital assets. The service works on the [!DNL Adobe I/O] runtime. It is extendable by creating custom workers using [!DNL Project Firefly]. These custom workers are (usually headless) [!DNL Project Firefly] apps and do tasks such as add custom conversion tools or call external APIs to perform image operations.

[!DNL Project Firefly] is a framework to build and deploy custom web applications on [!DNL Adobe I/O] runtime. To create custom applications, the developers can leverage [!DNL React Spectrum] (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs. See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

**TBD:**

* Add an introduction, PM overview.

## What Compute Service can do for you {#possible-use-cases-benefits}

The Asset Compute Service supports standard use cases like basic image processing; Adobe application specific conversions; and custom workers creation that orchestrate complex business requirements.

You can use [!DNL Asset Compute] web service to generate thumbnails for different file types, high-quality image renderings for the [supported file formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html), extract metadata, extract full text as precursor for indexing..

You can easily add custom conversion tools, say create composition workflows that combine [!DNL Adobe Sensei] results with image operations.

The service does not do the following on its own, by default.

|Asset Compute Service does not do this|Expectations from implementing client|
|---|---|
| Binary uploads or API-based asset ingestion. | Use other methods to ingest assets. |
| Store binaries or any persisted data across processing requests.| Each request is independent so treat it as a standalone request by sharing binary and processing instructions. |
| Store any configurations such as processing rules or settings for a user or an organization's account. | Add processing request to each request/instruction. |
| Direct event handling of asset creation events from storage systems and processing completed notifications, and errors. | Use Adobe I/O Events and other methods. |

**TBD:**

This section can be updated with the following information:

* Some info about the benefits is at https://git.corp.adobe.com/nui/nui/blob/master/whitepaper/Project-Nui-Asset-Compute-Service.md#benefits-for-aem-assets. See what's worth including.
* Explain extensibility and top-level functional use cases.
* Explain a few specific functional use cases as one-liners to illustrate a functional needs that Asset Compute Service can fulfill.

## Prerequisites {#prerequisites}

See [prerequisites in the release notes](release-notes.md#prerequisites).

>[!MORELIKETHIS]
>
>* [Overview of asset processing with asset microservices in Adobe Experience Manager as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/asset-microservices-overview.html).
>* [Documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).
>* [File formats supported for processing](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html).
>* [Release notes of the Asset Compute Service](release-notes.md)
