---
title: "WhiteboxTools v1.4.0 released"
datePublished: Mon Sep 07 2020 13:42:52 GMT+0000 (Coordinated Universal Time)
cuid: ckeskrpo70017kls12zclekm4
slug: whiteboxtools-v140-released
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1599486073385/KBWWm21ob.png
tags: gis, remote-sensing, geospatial

---

WhiteboxTools  [v1.4.0](https://github.com/jblindsay/whitebox-tools/releases/tag/1.4.0)  with **447** tools for geospatial analysis has been released by Dr. [John Lindsay](https://github.com/jblindsay). I have updated the WhiteboxTools frontends (Python, R, ArcGIS) to use WBT v1.4.0. Note that the QGIS frontend is not maintained by me. Therefore, it might not be up-to-date. 

- WhiteboxTools: [https://github.com/jblindsay/whitebox-tools](https://github.com/jblindsay/whitebox-tools)
- User Mannual: [https://jblindsay.github.io/wbt_book](https://jblindsay.github.io/wbt_book)
- whitebox-python: [https://github.com/giswqs/whitebox-python](https://github.com/giswqs/whitebox-python)
- whiteboxR: [https://github.com/giswqs/whiteboxR](https://github.com/giswqs/whiteboxR)
- whitebox-ArcGIS: [https://github.com/giswqs/WhiteboxTools-ArcGIS](https://github.com/giswqs/WhiteboxTools-ArcGIS)
- whitebox-QGIS: [https://jblindsay.github.io/wbt_book/qgis_plugin.html](https://jblindsay.github.io/wbt_book/qgis_plugin.html)

See below the updates for  [WhiteboxTools v1.4.0](https://github.com/jblindsay/whitebox-tools/releases/tag/1.4.0) :

This release adds some new tools and several bug fixes. Of the new tools, we are most excited about the addition of the [TimeInDaylight](https://jblindsay.github.io/wbt_book/available_tools/geomorphometric_analysis.html#TimeInDaylight) tool, which estimates the proportion of daytime that each cell in a digital surface model (DSM) is in an exposed area. This is a very powerful method of performing a basic solar radiation modelling workflow. The new [LidarDigitalSurfaceModel](https://jblindsay.github.io/wbt_book/available_tools/lidar_tools.html#LidarDigitalSurfaceModel) tool can be used to create DSMs for input to the TimeInDaylight tool. In addition to these updates, the User Manual has also been updated in numerous places with enhanced documentation. As always, if you wish to compile from source code, be sure to update to the most recent version of Rust beforehand using rustup update stable. Pre-compiled binaries are available for Windows (win), MacOS (Darwin), and Linux on Github and from the[ WhiteboxTools homepage](https://jblindsay.github.io/ghrg/WhiteboxTools/download.html). Please report any bugs that you encounter by creating an issue on the [WhiteboxTools Github site](https://github.com/jblindsay/whitebox-tools).

* Added the TimeInDaylight model tool for modelling the proportion of daytime that a location is not in shadow.
* Added the MapOffTerrainObjects tool.
* Added the FilterRasterFeaturesByArea tool.
* Added the LidarDigitalSurfaceModel tool.
* The D8 and FD8 flow pointer tools now output byte rasters.
* The Isobasins tool now optionally outputs an upstream/downstream connections table.
* The HorizonAngle tool has had significant performance improvements.
* Improvements to the RemoveOffTerrainObjects tool's performance.
* The Resample tool has been modified so that it does not require a 'destination' raster. Instead,
* it will create a new output raster either based on a user-specified target cell resolution or
* an optional base raster, much like the vector-to-raster conversion tools.
* Tools that input a z_factor conversion no longer override user input with geographic coordinates
* (see issue [#113](https://github.com/jblindsay/whitebox-tools/issues/113)).
* The StreamLinkIdentifier tool now outputs a 32-bit integer format, increasing the maximum allowable
* number of streams (see issue [#110](https://github.com/jblindsay/whitebox-tools/issues/110)).
* Fixed a bug with cubic-convolution and bilinear resampling in the Mosaic tool (see issue [#109](https://github.com/jblindsay/whitebox-tools/issues/109)).

**whitebox Python Runner**
![whitebox](https://wetlands.io/file/images/whitebox.png)
**whitebox for RStudio**
![whiteboxR](https://wetlands.io/file/images/whiteboxR.png)
**whitebox for ArcMap**
![whiteboxArcGIS](https://wetlands.io/file/images/whitebox-ArcGIS.png)
**whitebox for ArcGIS Pro**
![whiteboxArcGISPro](https://wetlands.io/file/images/whitebox-ArcGISPro.png)
**whitebox for QGIS**
![whiteboxQGIS](https://wetlands.io/file/images/whitebox-QGIS.png)