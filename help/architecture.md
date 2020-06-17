---
title: Introduction to the Asset Compute Service.
description: Asset Compute Service is a cloud-native asset processing service that helps reduce complexity and improves scalability.
---

# Architecture of Asset Compute Service {#overview}

TBD:

* Explain important transactions/handshakes?
* Flow of assets/control? See the illustration on Nui-diagrams wiki.
* Illustrations. See the SVG shared by Alex.
* Exceptions? Limitations? Call-outs? Gotchas?
  * Added elsewhere what the service does not do [here](introduction.md#possible-use-cases-benefits).
  * Do we want to add what basic processing is not available currently, that is expected by existing AEM customers?

___________

The Asset Compute Service is built on top of serverless Adobe I/O Runtime platform. It provides Adobe Sensei content services support for assets, where a JSON file is returned with one or more client-chosen, Adobe Sensei-generated describing a single asset.

Asset Compute Service is extendable by creating custom workers based on [!DNL Project Firefly]. These custom workers are [!DNL Project Firefly] apps and do tasks such as add custom conversion tools or call external APIs to perform image operations.

[!DNL Project Firefly] is a framework to build and deploy custom web applications on [!DNL Adobe I/O] runtime. To create custom applications, the developers can leverage [!DNL React Spectrum] (Adobe’s UI toolkit), create microservices, create custom events, and orchestrate APIs. See [documentation of Project Firefly](https://www.adobe.io/apis/experienceplatform/project-firefly/docs.html).

The foundation on which the architecture is based includes:

* Container technology allows to wrap existing rendering and processing technology (applications, libraries and even specialized web servers) using repeatable and automatable build instructions with minimal effort (Docker file). The modularity of containers – only containing what is needed for a given task – allows to decouple workers from each other and keep them lightweight. In exceptional cases, Virtual Machines automatically started in a cluster could also be used to wrap technology not yet container-ready.

* The serverless concept of Adobe I/O Runtime yields numerous benefits: asynchronous, highly-scalable, isolated, job-based processing, which is a perfect fit for asset processing. The ability to script in different languages with execution isolation allows for extensibility with 3rd party customer code. Minimal ramp up time, simple development & deployment are very helpful in making it a cost effective and attractive solution for Adobe teams, customers and partners.

* Binary cloud storage provides the necessary features for storing and accessing asset files and renditions individually, without requiring full access permissions to the storage, using pre-signed URL references. Storage notifications and event queues allow for simple integration in an asynchronous environment. Transfer acceleration, CDN caching, and co-locating compute workers with cloud storage allow for optimal low latency content access.

The architecture is compatible with Adobe Experience Data Model (XDM) Schema that ensures forward and backward compatibility.[`xmp:machineKeywords`](https://github.com/adobe/xdm/blob/master/docs/reference/assets/asset.schema.md#xmpmachinekeywords) tracks Adobe Sensei-assigned descriptive phrases such as keywords or tags of a supported asset.

![Architecture of Asset Compute Service](assets/architecture-diagram.png)

*Figure: Architecture of Asset Compute Service and how it integrates with Experience Manager, storage, and processing workers.*

The architecture consists of the following parts:

* **An API and orchestration layer** receives requests (in JSON format) which instruct the service to transform a source asset (or more) into multiple renditions. These requests are asynchronous and return with an activation id (aka "job id"). Instructions are purely declarative, and for all standard processing work (e.g. thumbnail generation, text extraction) consumers only specify the desired result, but not the workers that handle certain renditions. This is considered an implementation detail, as it might change over time or vary for different input file types. Generic API features such as authentication, analytics, rate limiting, are handled using the Adobe API Gateway in front of the service and manages all requests going to I/O Runtime. The worker routing is done dynamically by the orchestration layer. This layer can consider additional factors, including feature flags for testing new worker implementations. Custom workers can be specified by clients for particular renditions and include custom parameters. Worker execution can be fully parallelized as they are separate serverless functions in I/O Runtime.

* **Workers to process assets** that specialize on certain types of file formats or target renditions (although their containers could be combined if that leads to a higher ratio of hot containers on I/O Runtime). Workers are a combination of: a container image, which includes the I/O Runtime server managing the script execution and the necessary tools, for example a command line application to generate thumbnails; and a script (action or function) that either invokes the tools on the container, invokes other actions on I/O Runtime (such as Sensei functions) or makes a request to external services. Conceptually, a worker is similar to the Unix pipe concept: an input file gets transformed into one or more output files. Actual worker scripts operate solely on the local filesystem.

* **A common worker library** handles common tasks like downloading the source file, uploading the renditions, error reporting, monitoring and so on. This is designed so that developing a worker stays as simple as possible, following the serverless idea, and can be restricted to local filesystem interactions. The library can be supplied in the various languages supported by I/O Runtime.

<!--
An example of the JSON file is:

```json
{
    "xmp:machineKeywords": [
        {
            "value": "tree",
            "confidence": 0.8567
        },
        {
            "value": "brown",
            "confidence": 0.752
        }
    ]
}
```
-->

## Know the basic terminology {#glossary}

TBD:

* What do we want to populate here?
* Project Firefly and its term.
* AEMaaCS: See https://docs.adobe.com/content/help/en/experience-manager-cloud-service/overview/introduction.html.

___________

## Understand the required files and folders {#understand-purpose-of-files-folders}

* About the YAML file?
* See https://github.com/AdobeDocs/project-firefly/blob/master/getting_started/first_app.md#5-anatomy-of-a-project-firefly-application

---
