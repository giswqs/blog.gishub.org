## Machine Learning with Earth Engine - Unsupervised Classification

This tutorial shows you how to perform unsupervised classification (e.g., KMeans clustering) in Earth Engine.

## Unsupervised classification algorithms available in Earth Engine

Source: https://developers.google.com/earth-engine/clustering

The `ee.Clusterer` package handles unsupervised classification (or clustering) in Earth Engine. These algorithms are currently based on the algorithms with the same name in [Weka](http://www.cs.waikato.ac.nz/ml/weka/). More details about each Clusterer are available in the reference docs in the Code Editor.

Clusterers are used in the same manner as classifiers in Earth Engine. The general workflow for clustering is:

1. Assemble features with numeric properties in which to find clusters.
2. Instantiate a clusterer. Set its parameters if necessary.
3. Train the clusterer using the training data.
4. Apply the clusterer to an image or feature collection.
5. Label the clusters.

The training data is a `FeatureCollection` with properties that will be input to the clusterer. Unlike classifiers, there is no input class value for an `Clusterer`. Like classifiers, the data for the train and apply steps are expected to have the same number of values. When a trained clusterer is applied to an image or table, it assigns an integer cluster ID to each pixel or feature.

![gee](https://i.imgur.com/IcBapEx.png)

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
- https://github.com/giswqs/geemap/blob/master/examples/notebooks/31_unsupervised_classification.ipynb

**Demo**
![demo](https://i.imgur.com/uNQfrFx.gif)

**Video**

%[https://youtu.be/k9MEy2awVJQ]

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
# point = ee.Geometry.Point([-122.4439, 37.7538])
point = ee.Geometry.Point([-87.7719, 41.8799])

image = ee.ImageCollection('LANDSAT/LC08/C01/T1_SR') \
    .filterBounds(point) \
    .filterDate('2019-01-01', '2019-12-31') \
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
props = geemap.image_props(image)
props.getInfo()
```


```python
props.get('IMAGE_DATE').getInfo()
```


```python
props.get('CLOUD_COVER').getInfo()
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


```python
# Make the training dataset.
training = image.sample(**{
#     'region': region,
    'scale': 30,
    'numPixels': 5000,
    'seed': 0,
    'geometries': True  # Set this to False to ignore geometries
})

Map.addLayer(training, {}, 'training', False)
Map
```

### Train the clusterer


```python
# Instantiate the clusterer and train it.
n_clusters = 5
clusterer = ee.Clusterer.wekaKMeans(n_clusters).train(training)
```

### Classify the image


```python
# Cluster the input using the trained clusterer.
result = image.cluster(clusterer)

# # Display the clusters with random colors.
Map.addLayer(result.randomVisualizer(), {}, 'clusters')
Map
```

### Label the clusters


```python
legend_keys = ['One', 'Two', 'Three', 'Four', 'ect']
legend_colors = ['#8DD3C7', '#FFFFB3', '#BEBADA', '#FB8072', '#80B1D3']

# Reclassify the map
result = result.remap([0, 1, 2, 3, 4], [1, 2, 3, 4, 5])

Map.addLayer(result, {'min': 1, 'max': 5, 'palette': legend_colors}, 'Labelled clusters')
Map.add_legend(legend_keys=legend_keys, legend_colors=legend_colors, position='bottomright')
Map
```

### Visualize the result


```python
print('Change layer opacity:')
cluster_layer = Map.layers[-1]
cluster_layer.interact(opacity=(0, 1, 0.1))
```

### Export the result

Export the result directly to your computer:


```python
import os
out_dir = os.path.join(os.path.expanduser('~'), 'Downloads')
out_file = os.path.join(out_dir, 'cluster.tif')
```


```python
geemap.ee_export_image(result, filename=out_file, scale=90)
```

Export the result to Google Drive:


```python
geemap.ee_export_image_to_drive(result, description='clusters', folder='export', scale=90)
```
