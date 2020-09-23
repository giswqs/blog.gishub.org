## Earth Engine Tutorial #37: How to use Earth Engine with pydeck for 3D terrain visualization

**Requirements**

- [earthengine-api](https://github.com/google/earthengine-api): a Python client library for calling the Google Earth Engine API.
- [pydeck](https://pydeck.gl/index.html): a WebGL-powered framework for visual exploratory data analysis of large datasets.
- [pydeck-earthengine-layers](https://github.com/UnfoldedInc/earthengine-layers/tree/master/py): a pydekc wrapper for Google Earth Engine. For documentation please visit this [website](https://earthengine-layers.com/).
- [Mapbox API key](https://pydeck.gl/installation.html#getting-a-mapbox-api-key): you will need this add basemap tiles to pydeck.

**Installation**

- conda create -n deck python
- conda activate deck
- conda install mamba -c conda-forge
- mamba install earthengine-api pydeck pydeck-earthengine-layers -c conda-forge
- jupyter nbextension install --sys-prefix --symlink --overwrite --py pydeck
- jupyter nbextension enable --sys-prefix --py pydeck

This example is adopted from [here](https://github.com/UnfoldedInc/earthengine-layers/blob/master/py/examples/terrain.ipynb). Credits to the developers of the [pydeck-earthengine-layers](https://github.com/UnfoldedInc/earthengine-layers) package. 

**Demo**
![](https://i.imgur.com/Gx7Y015.gif)

**Video**

%[https://youtu.be/4E3zOP3-md8]

## 2D Visualization


```python
import ee
import geemap
```


```python
Map = geemap.Map()
Map
```


```python
image = ee.Image('USGS/NED').select('elevation')
vis_params={
    "min": 0, 
    "max": 4000,
    "palette": ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']
}
Map.addLayer(image, vis_params, 'NED')
```

## 3D Visualization

### Import libraries

First, import required packages. Note that here we import the `EarthEngineTerrainLayer` instead of the `EarthEngineLayer`.


```python
import ee
import pydeck as pdk
from pydeck_earthengine_layers import EarthEngineTerrainLayer
```

### Authenticate with Earth Engine

Using Earth Engine requires authentication. If you don't have a Google account approved for use with Earth Engine, you'll need to request access. For more information and to sign up, go to https://signup.earthengine.google.com/.


```python
try:
    ee.Initialize()
except Exception as e:
    ee.Authenticate()
    ee.Initialize()
```

### Terrain Example

In contrast to the `EarthEngineLayer`, where you need to supply _one_ Earth Engine object to render, with the `EarthEngineTerrainLayer` you must supply **two** Earth Engine objects. The first is used to render the image in the same way as the `EarthEngineLayer`; the second supplies elevation values used to extrude terrain in 3D. Hence the former can be any `Image` object; the latter must be an `Image` object whose values represents terrain heights.

It's important for the terrain source to have relatively high spatial resolution. In previous examples, we used [SRTM90][srtm90] as an elevation source, but that only has a resolution of 90 meters. When used as an elevation source, it looks very blocky/pixelated at high zoom levels. In this example we'll use [SRTM30][srtm30] (30-meter resolution) as the `Image` source and the [USGS's National Elevation Dataset][ned] (10-meter resolution, U.S. only) as the terrain source. SRTM30 is generally the best-resolution worldwide data source available.

[srtm90]: https://developers.google.com/earth-engine/datasets/catalog/CGIAR_SRTM90_V4
[srtm30]: https://developers.google.com/earth-engine/datasets/catalog/USGS_SRTMGL1_003
[ned]: https://developers.google.com/earth-engine/datasets/catalog/USGS_NED


```python
# image = ee.Image('USGS/SRTMGL1_003')
image = ee.Image('USGS/NED').select('elevation')
terrain = ee.Image('USGS/NED').select('elevation')
```

Here `vis_params` consists of parameters that will be passed to the Earth Engine [`visParams` argument][visparams]. Any parameters that you could pass directly to Earth Engine in the code editor, you can also pass here to the `EarthEngineLayer`.

[visparams]: https://developers.google.com/earth-engine/image_visualization


```python
vis_params={
    "min": 0, 
    "max": 4000,
    "palette": ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']
}
```

Now we're ready to create the Pydeck layer. The `EarthEngineLayer` makes this simple. Just pass the Earth Engine object to the class.

Including the `id` argument isn't necessary when you only have one pydeck layer, but it is necessary to distinguish multiple layers, so it's good to get into the habit of including an `id` parameter.


```python
ee_layer = EarthEngineTerrainLayer(
    image,
    terrain,
    vis_params,
    id="EETerrainLayer"
)
```

Then just pass this layer to a `pydeck.Deck` instance, and call `.show()` to create a map:


```python
view_state = pdk.ViewState(
    latitude=36.15,
    longitude=-111.96,
    zoom=10.5, 
    bearing=-66.16,
    pitch=60)

r = pdk.Deck(
    layers=[ee_layer], 
    initial_view_state=view_state
)

r.show()
```

