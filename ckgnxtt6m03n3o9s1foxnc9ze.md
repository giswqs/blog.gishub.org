---
title: "GEE Tutorial #40 - How to add interactive widgets to the map"
datePublished: Sat Oct 24 2020 17:08:59 GMT+0000 (Coordinated Universal Time)
cuid: ckgnxtt6m03n3o9s1foxnc9ze
slug: gee-tutorial-40-how-to-add-interactive-widgets-to-the-map
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603557595111/dAJA8ZUG4.png
tags: python, gis, remote-sensing, earth-engine, geemap

---

This tutorial shows you how to add interactive widgets to the map. 

**GitHub:** https://github.com/giswqs/geemap

**Notebook: **
https://github.com/giswqs/geemap/blob/master/examples/notebooks/40_ipywidgets.ipynb

**Demo:**
![](https://i.imgur.com/peRZZjj.gif)

**Video:**

%[https://youtu.be/KsIxGq6cHtw]


## Import libraries


```python
import ee
import geemap
import ipyleaflet
import ipywidgets as widgets
```

## Create an interactive map


```python
Map = geemap.Map(center=[40, -100], zoom=4)
Map
```

## Add Earth Engine data

### Add raster data


```python
dem = ee.Image('USGS/SRTMGL1_003')

vis_params = {
  'min': 0,
  'max': 4000,
  'palette': ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']}

Map.addLayer(dem, vis_params, 'DEM')
```

### Add vector data


```python
fc = ee.FeatureCollection('TIGER/2018/States')
Map.addLayer(fc, {}, 'US States')
```

## Change layer opacity


```python
Map
```


```python
dem_layer = Map.find_layer('DEM')
dem_layer.interact(opacity=(0, 1, 0.1))
```


```python
vector_layer = Map.find_layer('US States')
vector_layer.interact(opacity=(0, 1, 0.1))
```

## Widget list

https://ipywidgets.readthedocs.io/en/latest/examples/Widget%20List.html

### Numeric widgets

#### IntSlider


```python
int_slider = widgets.IntSlider(
    value=2000,
    min=1984,
    max=2020,
    step=1,
    description='Year:'
)
int_slider
```


```python
int_slider.value
```

#### FloatSlider


```python
float_slider = widgets.FloatSlider(
    value=0,
    min=-1,
    max=1,
    step=0.05,
    description='Threshold:'
)
float_slider
```


```python
float_slider.value
```

#### IntProgress


```python
int_progress = widgets.IntProgress(
    value=7,
    min=0,
    max=10,
    step=1,
    description='Loading:',
    bar_style='', # 'success', 'info', 'warning', 'danger' or ''
    orientation='horizontal'
)
int_progress
```


```python
int_text = widgets.IntText(
    value=7,
    description='Any:',
)
int_text
```


```python
float_text = widgets.FloatText(
    value=7.5,
    description='Any:',
)
float_text
```

### Boolean widgets

#### ToggleButton


```python
toggle_button = widgets.ToggleButton(
    value=False,
    description='Hello me',
    disabled=False,
    button_style='success', # 'success', 'info', 'warning', 'danger' or ''
    tooltip='Description',
#     icon='check' # (FontAwesome names without the `fa-` prefix)
)
toggle_button
```


```python
toggle_button.value
```

#### Checkbox


```python
checkbox = widgets.Checkbox(
    value=False,
    description='Check me',
    disabled=False,
    indent=False
)
checkbox
```


```python
checkbox.value
```

### Selection widgets

#### Dropdown


```python
dropdown = widgets.Dropdown(
    options=['USA', 'Canada', 'Mexico'],
    value='Canada',
    description='Country:'
)
dropdown
```


```python
dropdown.value
```

#### RadioButtons


```python
radio_buttons = widgets.RadioButtons(
    options=['USA', 'Canada', 'Mexico'],
    value='Canada',
    description='Country:'
)
radio_buttons
```


```python
radio_buttons.value
```

### String widgets

#### Text


```python
text = widgets.Text(
    value='USA',
    placeholder='Enter a country name',
    description='Country:',
    disabled=False
)
text
```


```python
text.value
```

#### Textarea


```python
widgets.Textarea(
    value='Hello World',
    placeholder='Type something',
    description='String:',
    disabled=False
)
```

#### HTML


```python
widgets.HTML(
    value="Hello <b>World</b>",
    placeholder='Some HTML',
    description='Some HTML',
)
```


```python
widgets.HTML(
    value='<img src="https://earthengine.google.com/static/images/earth-engine-logo.png" width="200" height="200">'
)
```

### Button


```python
button = widgets.Button(
    description='Click me',
    button_style='success', # 'success', 'info', 'warning', 'danger' or ''
    tooltip='Click me',
#     icon='check' # (FontAwesome names without the `fa-` prefix)
)
button
```

### Date picker


```python
date_picker = widgets.DatePicker(
    description='Pick a Date',
    disabled=False
)
date_picker
```


```python
date_picker.value
```

### Color picker


```python
color_picker = widgets.ColorPicker(
    concise=False,
    description='Pick a color',
    value='blue',
    disabled=False
)
color_picker
```


```python
color_picker.value
```

### Output widget


```python
out = widgets.Output(layout={'border': '1px solid black'})
out
```


```python
with out:
    for i in range(10):
        print(i, 'Hello world!')
```


```python
from IPython.display import YouTubeVideo
out.clear_output()
with out:
    display(YouTubeVideo('7qRtsTCnnSM'))
out
```


```python
out.clear_output()
with out:
    display(widgets.IntSlider())
out
```

## Add a widget to the map


```python
Map = geemap.Map()

dem = ee.Image('USGS/SRTMGL1_003')
fc = ee.FeatureCollection('TIGER/2018/States')
vis_params = {
  'min': 0,
  'max': 4000,
  'palette': ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']}

Map.addLayer(dem, vis_params, 'DEM')
Map.addLayer(fc, {}, 'US States')

Map
```


```python
output_widget = widgets.Output(layout={'border': '1px solid black'})
output_control = ipyleaflet.WidgetControl(widget=output_widget, position='bottomright')
Map.add_control(output_control)
```


```python
print('Nice map!')
```


```python
with output_widget:
    print('Nice map!')
    print('Hello')
```


```python
output_widget.clear_output()
logo = widgets.HTML(
    value='<img src="https://earthengine.google.com/static/images/earth-engine-logo.png" width="100" height="100">'
)
with output_widget:
    display(logo)
```


```python
def handle_interaction(**kwargs):
    latlon = kwargs.get('coordinates')
    if kwargs.get('type') == 'click':
        Map.default_style = {'cursor': 'wait'}
        xy = ee.Geometry.Point(latlon[::-1])
        selected_fc = fc.filterBounds(xy)
        
        with output_widget:
            output_widget.clear_output()
        
            try:
                name = selected_fc.first().get('NAME').getInfo()
                usps = selected_fc.first().get('STUSPS').getInfo()
                Map.layers = Map.layers[:4]        
                geom = selected_fc.geometry()
                layer_name = name + '-' + usps
                Map.addLayer(ee.Image().paint(geom, 0, 2), {'palette': 'red'}, layer_name)  
                print(layer_name)
            except Exception as e:
                print('No feature could be found')
                Map.layers = Map.layers[:4]
            
        Map.default_style = {'cursor': 'pointer'}

Map.on_interaction(handle_interaction)
```


