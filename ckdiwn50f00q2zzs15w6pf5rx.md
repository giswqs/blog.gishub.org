## Earth Engine Tutorial #30: How to get image properties and descriptive statistics

This tutorial shows you how to retrieve basic properties (metadata) of an Earth Engine image and how to calculate descriptive statistics (e.g., min, max, mean, std, sum). 

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
- https://github.com/giswqs/geemap/blob/master/examples/notebooks/30_image_props_stats.ipynb

**Demo**
![demo](https://i.imgur.com/3B6YhkI.gif)

**Video**

%[https://youtu.be/eixBPPWgWs8]

## Import libraries
```python
import ee
import geemap
```

## Create an interactive map

```python
Map = geemap.Map()
Map
```

## Add images to the map


```python
centroid = ee.Geometry.Point([-122.4439, 37.7538])

landsat = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR') \
    .filterBounds(centroid) \
    .first()

landsat_vis = {
    'min': 0,
    'max': 3000,
    'bands': ['B5', 'B4', 'B3']
}

Map.centerObject(centroid, 8)
Map.addLayer(landsat, landsat_vis, "Landsat-8")
```


```python
naip = ee.ImageCollection('USDA/NAIP/DOQQ') \
    .filterBounds(centroid) \
    .first()

naip_vis = {
    'bands': ['N', 'R', 'G']
}

Map.addLayer(naip, naip_vis, 'NAIP')
```

## Get image property names


```python
landsat.propertyNames().getInfo()
```


```python
landsat.get('CLOUD_COVER').getInfo()
```


```python
# The number of milliseconds since 1970-01-01T00:00:00Z.
landsat.get('system:time_start').getInfo()
```


```python
ee.Date(landsat.get('system:time_start')).format('YYYY-MM-dd').getInfo()
```

## Get image properties all at once


```python
landsat_props = geemap.image_props(landsat)
landsat_props.getInfo()
```


```python
landsat_props.get('IMAGE_DATE').getInfo()
```


```python
landsat_props.get('CLOUD_COVER').getInfo()
```


```python
naip_props = geemap.image_props(naip)
naip_props.getInfo()
```


```python
naip_props.get('NOMINAL_SCALE').getInfo()
```

## Get image descriptive statistics

Including minimum, maximum, mean, standard deviation, and sum.


```python
landsat_stats = geemap.image_stats(landsat, scale=90)
landsat_stats.getInfo()
```


```python
naip_stats = geemap.image_stats(naip, scale=10)
naip_stats.getInfo()
```
