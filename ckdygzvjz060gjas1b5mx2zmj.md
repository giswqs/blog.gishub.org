---
title: "Earth Engine Tutorial #34: Interactive extraction of pixel values and interactive region reduction"
datePublished: Mon Aug 17 2020 12:04:09 GMT+0000 (Coordinated Universal Time)
cuid: ckdygzvjz060gjas1b5mx2zmj
slug: earth-engine-tutorial-34-interactive-extraction-of-pixel-values-and-interactive-region-reduction
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1597623664321/eIpMokq5u.png
tags: python, gis, remote-sensing, geospatial, earth-engine

---

This tutorial shows you how to extract pixel values from Earth Engine images interactively and save pixel values as a csv and shapefile. It will also show you how to perform interactive region reduction, which allows users to draw a rectangle or circle to compute descriptive statistics (e.g., mean, min, max, std, sum) of pixels within a user-drawn geometry. The region reduction results can be exported as a csv for shapefile. 


## Requirements
- [geemap](https://github.com/giswqs/geemap) - A Python package for interactive mapping with Google Earth Engine, ipyleaflet, and ipywidgets

## Installation
```
conda create -n gee python=3.7
conda activate gee
conda install mamba -c conda-forge
mamba install geemap -c conda-forge
```

## Resources

**Notebook:**
- https://github.com/giswqs/geemap/blob/master/examples/notebooks/34_extract_values.ipynb

**Demo**
![demo](https://i.imgur.com/LXRqSTu.gif)

**Video**

%[https://youtu.be/VO6-uG7Dl7s]

## Step-by-step tutorial

## Interactive extraction of pixel values

### Import libraries


```python
import os
import ee
import geemap
```

### Create an interactive map


```python
Map = geemap.Map()
Map
```

### Add data to the map


```python
landsat7 = ee.Image('LE7_TOA_5YEAR/1999_2003') \
    .select([0, 1, 2, 3, 4, 6])
landsat_vis = {
    'bands': ['B4', 'B3', 'B2'], 
    'gamma': 1.4
}
Map.addLayer(landsat7, landsat_vis, "LE7_TOA_5YEAR/1999_2003")

Map.set_plot_options(add_marker_cluster=True)
```

### Activate the plotting tool

Tick the `Plotting` checkbox and click the mouse on the map to start displaying charts.

### Export pixel values to shapefile/csv


```python
out_dir = os.path.join(os.path.expanduser('~'), 'Downloads')
# out_csv = os.path.join(out_dir, 'points.csv')
out_shp = os.path.join(out_dir, 'points.shp')
```


```python
Map.extract_values_to_points(out_shp)
```

## Interactive Region Reduction

### Import libraries


```python
import os
import ee
import geemap
```

### Create an interactive map


```python
m = geemap.Map()
```

### Add add to the map


```python
collection = ee.ImageCollection('MODIS/006/MOD13A2') \
    .filterDate('2015-01-01', '2019-12-31') \
    .select('NDVI')

# Convert the image collection to an image.
image = collection.toBands()

ndvi_vis = {
  'min': 0.0,
  'max': 9000.0,
  'palette': [
    'FFFFFF', 'CE7E45', 'DF923D', 'F1B555', 'FCD163', '99B718', '74A901',
    '66A000', '529400', '3E8601', '207401', '056201', '004C00', '023B01',
    '012E01', '011D01', '011301'
  ]
}

m.addLayer(image, {}, 'MODIS NDVI Time-series')
m.addLayer(image.select(0), ndvi_vis, 'MODIS NDVI VIS')

m
```

### Set reducer


```python
m.set_plot_options(add_marker_cluster=True, marker=None)
m.roi_reducer = ee.Reducer.mean()
```

### Export data


```python
out_dir = os.path.join(os.path.expanduser('~'), 'Downloads')
# out_csv = os.path.join(out_dir, 'points.csv')
out_shp = os.path.join(out_dir, 'ndvi.shp')
m.extract_values_to_points(out_shp)
```

