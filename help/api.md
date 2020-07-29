---
title: Best practices to use Asset Compute Service.
description: Best practices, limitations, and tips to create custom workers using Asset Compute Service.
---

# Asset Compute Service HTTP API {#asset-compute-http-api}

The article describes the HTTP API. A high-level flow for clients of the service is below:

1. Client is provisioned as Adobe Developer Console integration in an IMS organization. Each separate system or environment has its own separate integration, in order to isolate the journal.

   >[!NOTE]
   >
   >In Adobe Experience Manager as a Cloud Service, it happens automatically, except for the stage environments.

2. Client generates an access token for the technical account using the [JWT (Service Account) Authentication](https://www.adobe.io/authentication/auth-methods.html).

3. Client calls [`/register`](#register) first to retrieve the journal URL. [`/register`](#register) needs to be called only once.

4. Client calls [`/process`](#process-request) for each asset for which it wants to generate N renditions. This is asynchronous.

5. Client regularly polls the journal to [receive events](#asynchronous-events) for each requested rendition when it has been successfully processed (`rendition_created` event type) or if there was an error (`rendition_failed` event type).

## Authentication and authorization {#authentication-and-authorization}

All APIs require access token authentication. The requests must set the following headers:

1. `Authorization` header with bearer token, which is the technical account token, received via [JWT exchange](https://www.adobe.io/authentication/auth-methods.html) from Adobe Developer Console integration. The [scopes](#scopes) are documented below.

<!-- TBD: Change the existing URL to a new path when a new path for docs is available. The current path contains master word that is not an inclusive term. Logged ticket in AIO's GitHub repo to get a new URL.
-->

2. `x-gw-ims-org-id` header with the IMS organization ID.

  >[!NOTE]
  >
  >Currently, set the `x-ims-org-id` header with the same org ID value.

3. `x-api-key` with the client ID from the Adobe Developers Console integration.

### Scopes {#scopes}

Ensure the following scopes for the access token:

* `openid`
* `AdobeID`
* `asset_compute`
* `read_organizations`
* `event_receiver`
* `event_receiver_api`
* `adobeio_api`
* `additional_info.roles`
* `additional_info.projectedProductContext`

These require the Adobe Developer Console integration to be subscribed to `Asset Compute`, `I/O Events`, and `I/O Management API` services. The breakdown of individual scopes is:

* Basic
  * scopes: `openid,AdobeID`

* Asset Compute
  * metascope: `asset_compute_meta`
  * scopes: `asset_compute,read_organizations`

* Adobe I/O Events
  * metascope: `event_receiver_api`
  * scopes: `event_receiver,event_receiver_api`

* Adobe I/O Management API
  * metascope: `ent_adobeio_sdk`
  * scopes: `adobeio_api,additional_info.roles,additional_info.projectedProductContext`

## Registration {#register}

For an existing technical account (Adobe Developer Console integration), create an Asset Compute service registration. This is required before the first request to `/process`. It returns the Event journal URL to receive asynchronous events.

For an existing technical account (Adobe Developer Console integration) that was setup for Asset Compute with `/register`, you can remove the setup and journal.

### Register request {#register-request}

| Parameter                | Value                                                |
|--------------------------|------------------------------------------------------|
| Method                   | `POST`                                               |
| Path                     | `/register`                                          |
| Header `Authorization`   | All [authorization related headers](#authentication-and-authorization). |
| Header `x-request-id`    | Optional, can be set by clients for a unique end-to-end identifier of the processing requests across systems. |

Ensure that the body of the request is empty.

### Register response {#register-response}

Response MIME type is `application/json`. Response header `X-Request-Id` is either the same as the `X-Request-Id` set in the request header or a uniquely generated one. Use for identifying requests across systems and for support requests.

The HTTP status codes are:

* **200 Success**: When the request is successful. It contains the `journal` URL that will be notified about any results of the asynchronous processing triggered via `/process` (as events type `rendition_created` when successful, or `rendition_failed` when failing).

  ```json
  {
      "ok": true,
      "journal": "https://api.adobe.io/events/organizations/xxxxx/integrations/xxxx/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "requestId": "1234567890"
  }
  ```

* **401 Unauthorized**: When the request does not have valid [authentication](#authentication-and-authorization). An example might be an invalid access token or invalid API key.
* **403 Forbidden**: When the request does not have valid [authorization](#authentication-and-authorization). An example might be a valid access token, but the Adobe Developer Console integration (technical account) is not subscribed to all required services.
* **429 Too many requests**: When the system is overloaded by this client or in general. Clients should retry with an [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff). The body will be empty.
* **4xx error**: When there was any other client error and registration failed. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

* **5xx error**: When there was any other server side error and registration failed. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

### Unregister request {#unregister-request}

| Parameter                | Value                                                |
|--------------------------|------------------------------------------------------|
| Method | `POST`          |
| Path   | `/unregister`   |
| Header `Authorization`   | All [authorization related headers](#authentication-and-authorization). |
| Header `x-request-id`    | Optional, can be set by clients for a unique end-to-end identifier of the processing requests across systems. |

The request body is empty.

### Unregister response {#unregister-response}

Response MIME type is `application/json`. Response header `X-Request-Id` is either the same as the `X-Request-Id` set in the request header or a uniquely generated one. Use for identifying requests across systems and for support requests.

The status codes are:

* **200 Success**: When the registration and journal is found and removed.

  ```json
  {
      "ok": true,
      "requestId": "1234567890"
  }
  ```

* **401 Unauthorized**: When the request does not have valid [authentication](#authentication-and-authorization). An example might be an invalid access token or invalid API key.
* **403 Forbidden**: When the request does not have valid [authorization](#authentication-and-authorization). An example might be a valid access token, but the Adobe Developer Console integration (technical account) is not subscribed to all required services.
* **404 Not found**: When there is no current registration for the given credentials.

  ```json
  {
      "ok": true,
      "requestId": "1234567890"
  }
  ```

* **429 Too many requests**: When the system is overloaded by this client or in general. Clients should retry with an [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff). The body will be empty.

* **4xx error**: When there was any other client error and unregister failed. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

* **5xx error**: When there was any other server side error and registration failed. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

## Asset processing {#asset-processing}

For a list of currently supported file formats, see [supported file formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html).

The `process` operation submits a job that will transform a source asset into multiple renditions, based on the instructions in the request. Notifications about successful completion (event type `rendition_created`) or any errors (event type `rendition_failed`) are sent to an Event journal that must be retrieved using [/register](#register) once before making any number of `/process` requests. Incorrectly formed requests immediately fail with a 400 error code.

### Process request {#process-request}

| Parameter                | Value                                                |
|--------------------------|------------------------------------------------------|
| Method    | `POST`       |
| Path      | `/process`   |
| Mime type | `application/json` |
| Header `Authorization`   | All [authorization related headers](#authentication-and-authorization). |
| Header `x-request-id`    | Optional, can be set by clients for a unique end-to-end identifier of the processing requests across systems. |

The request body must be in JSON format. It provides instructions on what asset to process and what renditions to generate.

Binaries are referenced using URLs, such as Amazon AWS S3 pre-signed URLs or Azure Blob Storage SAS URLs, for both reading the `source` asset (`GET` URLs) and writing the renditions (`PUT` URLs). The client is responsible for generating these pre-signed URLs.

#### Process request JSON fields {#process-request-json-fields}

```json
{
    "source": "", // source url
    "renditions" : [], // array with 1..N requested renditions, including target locations
    "userData": {} // custom user data from the client for events
}
```

The available fields are:

| Name         | Type     | Description | Example |
|--------------|----------|-------------|---------|
| `source`     | `string` | URL of the source asset to process. Optional based on requested rendition format (e.g. `fmt=zip`). | `"http://example.com/image.jpg"` |
| `source`     | `object` | Describing the source asset to process. See description of [Source object fields](#source-object-fields) below. Optional based on requested rendition format (e.g. `fmt=zip`). | `{"url": "http://example.com/image.jpg", "mimeType": "image/jpeg" }` |
| `renditions` | `array`  | Renditions to generate from the source file. Each rendition object supports [rendition instruction](#rendition-instructions). Required. | `[{ "target": "https://....", "fmt": "png" }]` |
| `userData`   | `string` | Object for passing custom client information that is sent to all events. For example, this allows for passing a custom asset ID to identify assets in events if that is not possible with the `source` URL itself. Optional. | `{ "my-asset-id": "0123456789" }` |

Note that `source` can either be a `<string>`, which is seen as a URL, or an `<object>` with additional field. The following variants are identical:

```json
"source": "http://example.com/image.jpg"
```

```json
"source": {
    "url": "http://example.com/image.jpg"
}
```

Note that `userData` is controlled by the client, and should not be modifed or used in the workers.

#### Source object fields {#source-object-fields}

| Name      | Type     | Description | Example |
|-----------|----------|-------------|---------|
| `url`     | `string` | URL of the source asset to process. Required. | `"http://example.com/image.jpg"` |
| `name`    | `string` | Source asset file name. File extension in the name might be used if no MIME type can be detected. Takes precedence over file name in URL path or file name in `content-disposition` header of the binary resource. Defaults to "file". | `"image.jpg"` |
| `size`    | `number` | Source asset file size in bytes. Takes precedence over `content-length` header of the binary resource. | `10234` |
| `mimetype`| `string` | Source asset file MIME type. Takes precedence over the `content-type` header of the binary resource. | `"image/jpeg"` |

#### A complete `process` request example {#complete-process-request-example}

```json
{
    "source": "https://www.adobe.com/content/dam/acom/en/lobby/lobby-bg-bts2017-logged-out-1440x860.jpg",
    "renditions" : [{
            "name": "image.48x48.png",
            "url": "https://some-presigned-put-url-for-image.48x48.png",
            "fmt": "png",
            "width": 48,
            "height": 48
        },{
            "name": "image.200x200.jpg",
            "url": "https://some-presigned-put-url-for-image.200x200.jpg",
            "fmt": "jpg",
            "width": 200,
            "height": 200
        },{
            "name": "cqdam.xmp.xml",
            "url": "https://some-presigned-put-url-for-cqdam.xmp.xml",
            "fmt": "xmp"
        },{
            "name": "cqdam.text.txt",
            "url": "https://some-presigned-put-url-for-cqdam.text.txt",
            "fmt": "text"
    }],
    "userData": {
        "my-asset-id": "1234567890"
    }
}
```

### Process response {#process-response}

| Parameter             | Value                                                |
|-----------------------|------------------------------------------------------|
| Mime type             | `application/json`                                   |
| Header `X-Request-Id` | Either the same as the `X-Request-Id` set in the header or a uniquely generated one. Use for identifying requests across systems and/or support requests. |

This request will return immediately with success or failure based on basic request validation. Actual asset processing will happen asynchronously.

Status codes:

* **200 Success**: If the request was successfully submitted. Response JSON includes `"ok": true`:

  ```json
  {
      "ok": true,
      "requestId": "1234567890",
      "activationId": "legacy-id"
  }
  ```

* **400 Invalid request**: If the request is incorrectly formed, such as required fields missing in the request JSON. Response JSON includes `"ok": false`:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

* **401 Unauthorized**: When the request does not have valid [authentication](#authentication-and-authorization). An example might be an invalid access token or invalid API key.
* **403 Forbidden**: When the request does not have valid [authorization](#authentication-and-authorization). An example might be a valid access token, but the Adobe Developer Console integration (technical account) is not subscribed to all required services.
* **429 Too many requests**: When the system is overloaded by this client or in general. Clients should retry with an [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff). The body will be empty.
* **4xx error**: When there was any other client error. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

* **5xx error**: When there was any other server side error. Usually a JSON response such as this is returned, although that is not guaranteed for all errors:

  ```json
  {
      "ok": false,
      "requestId": "1234567890",
      "message": "error message"
  }
  ```

Most clients are likely inclined to retry the exact same request with [exponential backoff](https://en.wikipedia.org/wiki/Exponential_backoff) on any error *except* configuration issues such as 401 or 403, or invalid requests like 400. Apart from regular rate limiting via 429 responses, a temporary service outage or limitation might result in 5xx errors. It would then be advisable to retry after a period of time.

All JSON responses (if present) include the `requestId` which is the same value as the `X-Request-Id` header. It is recommended to read from the header, since it is always present. The `requestId` is also returned in all I/O events related to processing requests as `requestId`. Clients must not make any assumption about the format of this string, it is an opaque string identifier.

For backwards compatibility with the beta API, an `activationId` is returned. It is deprecated and will be removed in the future. New clients instead use `requestId`/`X-Request-Id`.

## Rendition instructions {#rendition-instructions}

These are the available options for the `renditions` array in [/process](#process-request).

<!-- Attention: Remove reference to git.corp.
-->

| Name              | Type     | Description | Example |
|-------------------|----------|-------------|---------|
| `fmt`             | `string` | The renditions target format, can also be `text` for text extraction and `xmp` for extracting XMP metadata as xml. See [supported formats](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/assets/file-format-support.html) | `png` |
| `target` or `url` | `string` | URL to which the generated rendition should be uploaded using HTTP PUT. | `http://w.com/img.jpg` |
| `target`          | `object` | Multipart pre-signed URL upload information for the generated rendition. This is for [AEM/Oak Direct Binary Upload](https://jackrabbit.apache.org/oak/docs/features/direct-binary-access.html) with this [multipart upload behavior](http://jackrabbit.apache.org/oak/docs/apidocs/org/apache/jackrabbit/api/binary/BinaryUpload.html).<br>Fields:<ul><li>`urls`: array of strings, one for each pre-signed part URL</li><li>`minPartSize`: the minimum size to use for one part = url</li><li>`maxPartSize`: the maximum size to use for one part = url</li></ul> | `{ "urls": [ "https://part1...", "https://part2..." ], "minPartSize": 10000, "maxPartSize": 100000 }` |
| `width`           | `number` | Width in pixels. only for image renditions. | `200` |
| `height`          | `number` | Height in pixels. only for image renditions. | `200` |
|                   |          |  - if only `width` or `height` is specified, the resulting image will use that and keep the aspect ratio<br> - without `width` and `height`, the original image pixel size is used. It depends on the source type. For some formats, such as PDF files, a default size is used. | |
| `quality`         | `number` | Specify jpeg quality in the range of `1` to `100`. Applicable only for image renditions. | `90` |
| `xmp`             | `string` | Used only by XMP metadata writeback, it is base64 encoded XMP to write back to the specified rendition. | |
| `interlace`       | `bool`   | Create interlaced PNG or GIF or progressive JPEG by setting it to true. It has no effect on other file formats. | |
| `jpegSize`        | `number` | Approximate size of JPEG file in bytes. Note this overrides any `quality` setting. Has no effect on other formats. | |
| `dpi`             | `number` or `object` | Set x and y dpi. For simplicity, it can also be set to a single number which will be used for both x and y. It has no effect on the image itself. | `96` or `{ xdpi: 96, ydpi: 96 }` |
| `convertToDpi`    | `number` or `object` | x and y dpi resample values while maintaining physical size. For simplicity, it can also be set to a single number which will be used for both x and y. | `96` or `{ xdpi: 96, ydpi: 96 }` |
| `files`           | `array`  | List of files to include in the ZIP archive (`fmt=zip`). Each entry can either be a URL string or an object with the fields:<ul><li>`url`: URL to download file</li><li>`path`: Store file under this path in the ZIP</li></ul> | `[{ "url": "https://host/asset.jpg", "path": "folder/location/asset.jpg" }]` |
| `duplicate`       | `string` | Duplicate handling for ZIP archives (`fmt=zip`). By default multiple files stored under the same path in the ZIP will generate an error. Setting `duplicate` to `ignore` will result in only the first asset to be stored and the rest to be ignored. | `ignore` |
| `worker`          | `string` | URL of a [custom worker](develop-custom-worker.md). Must be an `https://` URL. If this field is present, the rendition is created by a custom worker. Any other set rendition field is then used in the custom worker. | `"https://1234.adobeioruntime.net`<br>`/api/v1/web`<br>`/example-custom-worker-master/worker"` |
| `*`               | `*`      | Advanced, custom fields can be added that a [custom worker](develop-custom-worker.md) understands. | |

## Asynchronous events {#asynchronous-events}

Once processing of a rendition is finished or when an error occurs, an event is sent to an [Adobe I/O Events Journal](https://www.adobe.io/apis/experienceplatform/events/documentation.html#!adobedocs/adobeio-events/master/intro/journaling_api.md). Clients must listen to the journal URL provided through [/register](#register). The journal response includes an `event` array consisting of one object for each event, of which the `event` field includes the actual event payload.

The Adobe I/O Event type for all events of the Asset Compute service is `asset_compute`. The journal is automatically subscribed to this event type only and there is no further requirement to filter based on the Adobe I/O Event type. The service specific event types are available in the `type` property of the event.

### Event types {#event-types}

| Event               | Description |
|---------------------|-------------|
| `rendition_created` | Sent for each successfully processed and uploaded rendition. |
| `rendition_failed`  | Sent for each rendition that failed to process or upload. |

### Event attributes {#Event-attributes}

| Attribute   | Type     | Event         | Description |
|-------------|----------|---------------|-------------|
| `date`      | `string` | `*`           | Timestamp when event was sent in simplified extended [ISO-8601](https://en.wikipedia.org/wiki/ISO_8601) format (as defined by [Javascript Date.toISOString()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toISOString)). |
| `requestId` | `string` | `*`           | The request id of the original request to `/process`, same as `X-Request-Id` header. |
| `source`    | `object` | `*`           | The `source` of the `/process` request. |
| `userData`  | `object` | `*`           | The `userData` of the `/process` request if set. |
| `rendition` | `object` | `rendition_*` | The corresponding rendition object passed in `/process`. |
| `metadata`  | `object` | `rendition_created` | The [metadata](#metadata) properties of the rendition. |
| `errorReason`  | `string` | `rendition_*` | Rendition failure [reason](#error-reasons) if any. |
| `errorMessage` | `string` | `rendition_*` | Text giving more detail about the rendition failure if any. |

### Metadata {#metadata}

| Property  | Description |
|--------|-------------|
| `tiff:ImageWidth`  | The width of the rendition in pixels.  Will not be present if the rendition is not an image. |
| `tiff:ImageLength` | The length of the rendition in pixels.  Will not be present if the rendition is not an image. |
| `repo:size` | The size of the rendition in bytes. |
| `repo:sha1` | The sha1 digest of the rendition. |

### Error reasons {#error-reasons}

| Reason  | Description |
|---------|-------------|
| `SourceFormatUnsupported`    | The source is of an unsupported type. |
| `RenditionFormatUnsupported` | The requested format is unsupported. |
| `SourceUnsupported` | The specific source is unsupported even though the type is supported. |
| `SourceCorrupt`     | The source data is corrupt.  Includes empty files. |
| `RenditionTooLarge` | The rendition could not be uploaded using the pre-signed URL(s) provided in `target`. The actual rendition size is available as metadata in `repo:size` and can be used by the client to re-process this rendition with the right number of pre-signed URLs. |
| `GenericError`      | Any other error. |
