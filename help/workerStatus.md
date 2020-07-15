---
title: Worker status
description: Worker status
---

# Worker status {#worker-status}

<!-- Attention: Do we want to include this information in the public docs. Once we have documented the supported file types from the formats MD file, is this information further useful to the devs?
-->

One of the current issues with the workers is do they support specifying any combination of width and/or height for a rendition with not specifying either means producing a full size rendition. The state of workers that create image renditions is below.

| Worker         | Handles all width/height combos | Has full set of test |
|----------------|---------------------------------|----------------------|
| CameraRaw      | Yes                             | Yes                  |
| DCX            | Yes, for 1:1 see comment below  | Yes                  |
| FFmpeg         | Yes                             | Yes                  |
| Flite          | Yes                             | Yes                  |
| GraphicsMagick | Yes                             | Yes                  |
| ImageMagick    | Yes                             | Yes                  |
| LibreOffice    | Yes                             | Yes                  |
| InDesign       | Yes, for 1:1 see comment below  | Yes                  |
| PDFRasterizer  | Yes                             | Yes                  |
| PIE            | Yes                             | Yes                  |

* For 3D files that DCX worker handles, 1:1 doesn't really mean much, so what is currently implemented is that it uses the size of the largest thumbnail in the file.
* For Adobe InDesign the worker does not know the size of the document, so for 1:1 it uses the size of the embedded thumbnail.
