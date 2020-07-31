---
title: Deploy Asset Compute Service custom worker.
description: Deploy Asset Compute Service custom worker.
---

# Deploy a custom application {#deploy-custom-worker}

Deploy your worker using `aio app deploy` command. This command returns a URL for your custom worker. Use the URL in a [Processing Profile in Experience Manager as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/manage/asset-microservices-configure-and-use.html) to integrate your worker. The URL is of the format:

`https://[namespace].adobeio-static.net/api/v1/web/[appname]-[appversion]/[workername]`

Ensure that your Firefly project and Workspace correspond to the Experience Manager as a Cloud Service environment where you want to use your action. It has three different environments for development, staging, and production. You can verify the environment by checking `AIO_runtime_*` credentials that are defined inside your ENV file in the root of your Firefly application. For instance, to deploy to a Stage workspace, the `AIO_runtime_namespace` is of the format `xxxxxx_xxxxxxxxx_stage`.


>[!MORELIKETHIS]
>
>* [Understand and manage environments in Experience Manager as a Cloud Service](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html).
