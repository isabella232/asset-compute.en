---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that reduces complexity and improves scalability.
---

# Overview of Asset Compute Service {#overview}

Asset Compute Service is a scalable and extensible service of Adobe Experience Cloud to process digital assets. It can transform image, video, document and other file formats into different renditions including thumbnails, extracted text & metadata and archives.

Developers can plugin custom asset workers to address custom use cases. The service works on the [!DNL Adobe I/O] runtime. It is extendable through [!DNL Project Firefly] headless apps written in Node.js. These can do custom operations such as calling external APIs to perform image operations or leverage [!DNL Adobe Sensei] support.

[!DNL Project Firefly] is a framework to build and deploy custom web applications on [!DNL Adobe I/O] runtime to extend Adobe Experience Cloud solutions. To create custom applications, the developers can leverage [!DNL React Spectrum] (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs. See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

**TBD:**

* Clarify the service can only be used within AEM as Cloud Service at this point
  - "can do for you" title below is misleading
  - docs provided as context for custom worker developers
  - and API as that will play a role in custom workers (accepting standard params, invoking Nui itself in the future, etc. (this is an outlook))
* link to aem as cloud service docs on asset ingestion & customization with processing profiles
* Add an introduction, PM overview.

## What Compute Service can do for you {#possible-use-cases-benefits}

The Asset Compute Service supports standard use cases like basic image processing; Adobe application specific conversions; and custom workers creation that orchestrate complex business requirements.

You can use [!DNL Asset Compute] web service to generate thumbnails for different file types, high-quality image renderings for the [supported file formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html), extract metadata, extract full text as precursor for indexing..

You can easily add custom conversion tools, say create composition workflows that combine [!DNL Adobe Sensei] results with image operations.

Note that the service does not provide any form of asset storage, which must be provided by consumers that provide references to source and rendition file locations in cloud storage. Furthermore, the service does not do the following on its own, by default.

|Asset Compute Service does not do this|Expectations from implementing client|
|---|---|
| Binary uploads or API-based asset ingestion. | Use other methods to ingest assets. |
| Store binaries or any persisted data across processing requests.| Each request is independent so treat it as a standalone request by sharing binary and processing instructions. |
| Store any configurations such as processing rules or settings for a user or an organization's account. | Add processing request to each request/instruction. |
| Direct event handling of asset creation events from storage systems and processing completed notifications, and errors. | Use Adobe I/O Events and other methods. |

**TBD:**

This section can be updated with the following information:

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
