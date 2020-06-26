One of the current issues with the workers is do they support specifying any combination of width and/or height for a rendition with not specifying either means producing a full size rendition.

Here is the current state as of October 28, 2019 of those workers that produce image renditions:
```
Worker              Handles all width/height combos      Has full set of tests   
------              ------------------------------       --------------------     
Cameraraw           Yes                                  Yes
DCX                 Yes, for 1:1 see comment below       Yes
Ffmpeg              Yes                                  Yes
Flite               Yes                                  Yes
GraphicsMagick      Yes                                  Yes
ImageMagick         Yes                                  Yes
Libreoffice         Yes                                  Yes
Indesign            Yes, for 1:1 see comment below       Yes
Pdfrasterizer       Yes                                  Yes
PIE                 Yes                                  Yes
```

For 3D files that DCX worker handles, 1 to 1 doesn't really mean much, so what is currently implemented is that it uses the size of the largest thumbnail in the file.

For Indesign the worker does not know the size of the document, so for 1 to 1 it uses the size of the embedded thumbnail.
