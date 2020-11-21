## GEE Tutorial #42 - How to upload data to Earth Engine Apps using ipywidgets

%[https://youtu.be/4-WeaiObj84]


```python
import os
import ee
import geemap
import ipywidgets as widgets
from ipyleaflet import WidgetControl
```


```python
Map = geemap.Map(center=(40, -100), zoom=4, height='750px')
```


```python
# Add Earth Engine dataset
dem = ee.Image('USGS/SRTMGL1_003')

# Set visualization parameters.
vis_params = {
  'min': 0,
  'max': 4000,
  'palette': ['006633', 'E5FFCC', '662A00', 'D8D8D8', 'F5F5F5']}

# Add Earth Eninge layers to Map
Map.addLayer(dem, vis_params, 'STRM DEM', True, 0.7)

Map
```


```python
output_widget = widgets.Output(layout={'border': '1px solid black'})
output_control = WidgetControl(widget=output_widget, position='bottomright')
Map.add_control(output_control)
```


```python
style = {'description_width': 'initial'}
uploader = widgets.FileUpload(
    description='Upload data',
    accept='.zip, .json, .geojson', 
    multiple=False,
    button_style='primary',
    style=style
)

submit = widgets.Button(
    description='Display data',
    button_style='success',
    tooltip='Click me',
    style=style)

reset = widgets.Button(
    description='Reset',
    button_style='warning',
    tooltip='Click me',
    style=style)
```


```python
with output_widget:
    print('Upload shapefile or \ngeojson as a zip file')
    display(uploader)
    display(submit)
    display(reset)
```


```python
def get_vector(upload_widget, out_dir=None):
    
    import zipfile
    import glob
    
    if out_dir is None:
        out_dir = os.path.join(os.path.expanduser('~'), 'Downloads')
    if not os.path.exists(out_dir):
        os.makedirs(out_dir)
        
    vector = None
    
    try:    
        [uploaded_file] = upload_widget.value
        file = upload_widget.value[uploaded_file]
        name = file['metadata']['name']
        content = file['content']
        out_file = os.path.join(out_dir, name)
        with open(out_file, "wb") as fp:
            fp.write(content)

        if name.endswith('.zip'):
            with zipfile.ZipFile(out_file, "r") as zip_ref:
                extract_dir = os.path.join(out_dir, name[:-4] + "_" + geemap.random_string(3))
                zip_ref.extractall(extract_dir)
                files = glob.glob(extract_dir + '/*.shp')
                if len(files) > 0:
                    shp = files[0]
                    vector = geemap.shp_to_ee(shp)
                else:
                    files = glob.glob(extract_dir + '/*.geojson')
                    if len(files) > 0:
                        geojson = files[0]
                        vector = geemap.geojson_to_ee(geojson)
        else:
            vector = geemap.geojson_to_ee(out_file)
                                
    except Exception as e:
        print(e)

    return vector
```


```python
def submit_clicked(b):
    if uploader._counter > 0:
        Map.default_style = {'cursor': 'wait'}        
        try:
            fc = get_vector(uploader)
            layer_name = 'Layer ' + geemap.random_string(3)
            Map.addLayer(fc, {}, layer_name)
            Map.centerObject(fc)
            uploader.value.clear()
            uploader._counter = 0    
        except Exception as e:
            print(e)
        Map.default_style = {'cursor': 'pointer'}

submit.on_click(submit_clicked)

def reset_clicked(b):

    Map.layers = Map.layers[:3]
    output_widget.clear_output()
    with output_widget:
        print('Upload shapefile or \ngeojson as a zip file')
        display(uploader)
        display(submit)
        display(reset)
    uploader.value.clear()
    uploader._counter = 0
    
reset.on_click(reset_clicked)
```


```python
Map.remove_layer()
```
