---
title: "How to configure Jupyter Notebook for ArcGIS"
datePublished: Sat Aug 01 2020 16:43:11 GMT+0000 (Coordinated Universal Time)
cuid: ckdbvx2e702mp66s1deus31mw
slug: how-to-configure-jupyter-notebook-for-arcgis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1596300125621/BH7xlmZnD.png
tags: python, gis

---

This is a tutorial for configuring Jupyter notebook for ArcGIS. 

* Download  [Anaconda **32-bit** Python 3.x version](https://www.anaconda.com/products/individual#windows) 
* Install Anaconda 32-bit to **C:\Anaconda3**
* Add Anaconda to Windows system PATH (Control Panel - System and Security - System - Advanced system settings - Advanced Tab - Environment Variables - System variables -  Double click Path variable - New - Add C:\Anaconda3\Scripts to Path variable) - OK
* Open ArcGIS Python Windows (ArcMap -- Geoprocessing -- Python)
* To find out the versions of Python and relevant packages ArcGIS is using, enter the following commands in the Python Window. Alternatively, you can go to this [webpage](https://support.esri.com/en/technical-article/000013224) to check the Python and numpy version for your ArcGIS Desktop. Record the version numbers for Python and numpy.
* `import sys, numpy`
* `print(sys.version, numpy.__version__)`
* Windows 10 Start Menu -- Anaconda3 (32-bit) -- Anaconda Prompt
* `conda create -n arcgis python=2.7.12 numpy=1.9.3  (Note: change the version numbers based on your ArcGIS version)`
* `conda activate arcgis`
* `conda install notebook ipykernel`
* `pip install archook`
* `ipython kernel install --user --name="arcgis"`
* Windows 10 Start Menu -- Anaconda3 (32-bit) -- Jupyter Notebook (arcgis)
* Run the following Python script to Jupyter Notebook and run the script to see if you get any errors.  


```python
try:
    import archook #The module which locates arcgis
    archook.get_arcpy()
    import arcpy
except ImportError:
    print("import arcpy error")
```


```python
d = arcpy.GetInstallInfo()
for key, value in list(d.items()):
    # Print a formatted string of the install key and its value
    #
    print("{:<13} : {}".format(key, value))
```

    SourceDir     : C:\Users\Administrator\Documents\ArcGIS 10.6\Desktop\SetupFiles\
    InstallDate   : 6/11/2018
    InstallDir    : c:\program files (x86)\arcgis\desktop10.6\
    ProductName   : Desktop
    BuildNumber   : 8321
    InstallType   : N/A
    Version       : 10.6
    SPNumber      : N/A
    Installer     : Administrator
    SPBuild       : N/A
    InstallTime   : 14:19:27



```python
print(arcpy.GetInstallInfo()['Version'])
```

    10.6