---
title: "How to create timelapse animations of spectral indices (e.g., NDWI, NDVI)"
datePublished: Sun Oct 11 2020 01:05:08 GMT+0000 (Coordinated Universal Time)
cuid: ckg4eo86l04mge9s18nt79m02
slug: how-to-create-timelapse-animations-of-spectral-indices-eg-ndwi-ndvi
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1602377988346/GA3NfLKrU.png
tags: python, gis, remote-sensing, earth-engine, geemap

---

You can now use one line of code to create a Landsat timelapse of a normalized difference index (e.g., NDVI, NDWI) using Earth Engine and geemap. This option will be added to the  [Landsat timelapse web app](https://geemap.herokuapp.com/voila/render/timelapse.ipynb)  soon. 

Notebook: https://gist.github.com/giswqs/bab47213f0cbd3d1073ea47b01331c62


The original Landsat timelapse (band combination: SWIR1/NIR/Red)
![](https://i.imgur.com/gNnQ2iH.gif)

Surface water area extracted using the Normalized Difference Water Index (NDWI)
![](https://i.imgur.com/Lh0krwz.gif)

Vegetation area extracted using the Normalized Difference Vegetation Index (NDVI)
![](https://i.imgur.com/C5cbBoC.gif)

Source code:

```python
import ee
import geemap
```

Install the development version from GitHub


```python
# geemap.update_package()
```


```python
Map = geemap.Map()
Map
```

Use the Drawing Tools to draw a rectangle.


```python
Map.setCenter(-74.520409, -8.373150, 12)
```


```python
Map.add_landsat_ts_gif(label= 'Pucallpa, Peru', bands=['SWIR1', 'NIR', 'Red'], nd_bands=['NIR', 'Red'], nd_palette=['black', 'green'], nd_threshold=0.3, start_year=2000, start_date='01-01', end_date='12-31', frames_per_second=5)
```
