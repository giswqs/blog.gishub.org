## Earth Engine Tutorial #29: How to use pydeck for rendering Earth Engine data

This video shows you how to use pydeck for rendering Earth Engine data. 

### Requirements
- [earthengine-api](https://github.com/google/earthengine-api): a Python client library for calling the Google Earth Engine API.
- [pydeck](https://pydeck.gl/index.html): a WebGL-powered framework for visual exploratory data analysis of large datasets.
- [pydeck-earthengine-layers](https://github.com/UnfoldedInc/earthengine-layers/tree/master/py): a pydekc wrapper for Google Earth Engine. For documentation please visit this [website](https://earthengine-layers.com/).
- [Mapbox API key](https://pydeck.gl/installation.html#getting-a-mapbox-api-key): you will need this add basemap tiles to pydeck.

### Installation

- conda create -n deck python
- conda activate deck
- conda install mamba -c conda-forge
- mamba install earthengine-api pydeck pydeck-earthengine-layers -c conda-forge
- jupyter nbextension install --sys-prefix --symlink --overwrite --py pydeck
- jupyter nbextension enable --sys-prefix --py pydeck
GitHub: https://github.com/giswqs/geemap

### Resources

**Notebook: **
- https://github.com/giswqs/geemap/blob/master/examples/notebooks/29_pydeck.ipynb

**Demo:**

![](https://i.imgur.com/HjFB95l.gif)

**Video:**

%[https://youtu.be/EIkEH4okFF4]