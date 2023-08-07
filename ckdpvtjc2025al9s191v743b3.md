---
title: "Earth Engine Tutorial #32: Machine Learning with Earth Engine - Supervised Classification"
datePublished: Tue Aug 11 2020 11:49:12 GMT+0000 (Coordinated Universal Time)
cuid: ckdpvtjc2025al9s191v743b3
slug: earth-engine-tutorial-32-machine-learning-with-earth-engine-supervised-classification
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1597112209274/8ilfgNOMY.png
tags: python, gis, remote-sensing, geospatial, earth-engine

---

This tutorial shows you how to perform supervised classification (e.g., Classification and Regression Trees [CART]) in Earth Engine.

# Supervised classification algorithms available in Earth Engine

Source: https://developers.google.com/earth-engine/classification

The `Classifier` package handles supervised classification by traditional ML algorithms running in Earth Engine. These classifiers include CART, RandomForest, NaiveBayes and SVM. The general workflow for classification is:

1. Collect training data. Assemble features which have a property that stores the known class label and properties storing numeric values for the predictors.
2. Instantiate a classifier. Set its parameters if necessary.
3. Train the classifier using the training data.
4. Classify an image or feature collection.
5. Estimate classification error with independent validation data.

The training data is a `FeatureCollection` with a property storing the class label and properties storing predictor variables. Class labels should be consecutive, integers starting from 0. If necessary, use remap() to convert class values to consecutive integers. The predictors should be numeric.

![gee](https://i.imgur.com/vROsEiq.png)

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
- https://github.com/giswqs/geemap/blob/master/examples/notebooks/32_supervised_classification.ipynb

**Demo**
![demo](https://i.imgur.com/jJ2Xiu6.gif)

**Video**

%[https://youtu.be/qWaEfgWi21o]

## Step-by-step tutorial

### Import libraries


```python
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
point = ee.Geometry.Point([-122.4439, 37.7538])
# point = ee.Geometry.Point([-87.7719, 41.8799])

image = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR') \
    .filterBounds(point) \
    .filterDate('2016-01-01', '2016-12-31') \
    .sort('CLOUD_COVER') \
    .first() \
    .select('B[1-7]')

vis_params = {
    'min': 0,
    'max': 3000,
    'bands': ['B5', 'B4', 'B3']
}

Map.centerObject(point, 8)
Map.addLayer(image, vis_params, "Landsat-8")
```

### Check image properties


```python
ee.Date(image.get('system:time_start')).format('YYYY-MM-dd').getInfo()
```


```python
image.get('CLOUD_COVER').getInfo()
```

### Make training dataset

There are several ways you can create a region for generating the training dataset.

- Draw a shape (e.g., rectangle) on the map and the use `region = Map.user_roi`
- Define a geometry, such as `region = ee.Geometry.Rectangle([-122.6003, 37.4831, -121.8036, 37.8288])`
- Create a buffer zone around a point, such as `region = ee.Geometry.Point([-122.4439, 37.7538]).buffer(10000)`
- If you don't define a region, it will use the image footprint by default


```python
# region = Map.user_roi
# region = ee.Geometry.Rectangle([-122.6003, 37.4831, -121.8036, 37.8288])
# region = ee.Geometry.Point([-122.4439, 37.7538]).buffer(10000)
```

In this example, we are going to use the [USGS National Land Cover Database (NLCD)](https://developers.google.com/earth-engine/datasets/catalog/USGS_NLCD) to create label dataset for training


![](https://i.imgur.com/7QoRXxu.png)


```python
nlcd = ee.Image('USGS/NLCD/NLCD2016').select('landcover').clip(image.geometry())
Map.addLayer(nlcd, {}, 'NLCD')
Map
```


```python
# Make the training dataset.
points = nlcd.sample(**{
    'region': image.geometry(),
    'scale': 30,
    'numPixels': 5000,
    'seed': 0,
    'geometries': True  # Set this to False to ignore geometries
})

Map.addLayer(points, {}, 'training', False)
```


```python
print(points.size().getInfo())
```


```python
print(points.first().getInfo())
```

### Train the classifier


```python
# Use these bands for prediction.
bands = ['B1', 'B2', 'B3', 'B4', 'B5', 'B6', 'B7']


# This property of the table stores the land cover labels.
label = 'landcover'

# Overlay the points on the imagery to get training.
training = image.select(bands).sampleRegions(**{
  'collection': points,
  'properties': [label],
  'scale': 30
})

# Train a CART classifier with default parameters.
trained = ee.Classifier.smileCart().train(training, label, bands)
```


```python
print(training.first().getInfo())
```

### Classify the image


```python
# Classify the image with the same bands used for training.
result = image.select(bands).classify(trained)

# # Display the clusters with random colors.
Map.addLayer(result.randomVisualizer(), {}, 'classfied')
Map
```

### Render categorical map

To render a categorical map, we can set two image properties: `landcover_class_values` and `landcover_class_palette`. We can use the same style as the NLCD so that it is easy to compare the two maps. 


```python
class_values = nlcd.get('landcover_class_values').getInfo()
class_values
```


```python
class_palette = nlcd.get('landcover_class_palette').getInfo()
class_palette
```


```python
landcover = result.set('classification_class_values', class_values)
landcover = landcover.set('classification_class_palette', class_palette)
```


```python
Map.addLayer(landcover, {}, 'Land cover')
Map
```

### Visualize the result


```python
print('Change layer opacity:')
cluster_layer = Map.layers[-1]
cluster_layer.interact(opacity=(0, 1, 0.1))
```

### Add a legend to the map


```python
Map.add_legend(builtin_legend='NLCD')
Map
```

### Export the result

Export the result directly to your computer:


```python
import os
out_dir = os.path.join(os.path.expanduser('~'), 'Downloads')
out_file = os.path.join(out_dir, 'landcover.tif')
```


```python
geemap.ee_export_image(landcover, filename=out_file, scale=900)
```

Export the result to Google Drive:


```python
geemap.ee_export_image_to_drive(landcover, description='landcover', folder='export', scale=900)
```