# RRaster
### Simple and readable raster manipulation with Python.

This package is an attempt to create a set of user-friendly functions for reprojecting and manipulating rasters with 
Python. The syntax is meant to mirror that of the R Raster library. 

Dependencies
------------
- `Python` 3.8+
- `numpy` 1.3+
- `rasterio` 1.2.6+
- `matplotlib` 3.4.1+

Installation
------------
It is recommended to use this package within a `virtualenv`.

```sh
$ python3 -m venv venv
$ source venv/bin/activate

# The RRaster package can be installed with pip
$ pip install rraster

# Alternatively, the development version of the package can be installed from 
$ pip install -r requirements.txt
$ pip install .
````

Examples
--------
The current operations supported by RRaster are reprojection, reduction, writing to disk, and basic raster calculations
(addition, subtraction, etc.). Below, a [gridMet](http://www.climatologylab.org/gridmet.html) precipitation raster is 
projected to the [EASE-2 grid](https://nsidc.org/data/ease) and some basic manipulations are done. Although this syntax
may be less Pythonic, I find it much easier to read and remember than the typical rasterio syntax.

```python
import numpy as np
from pathlib import Path
import pkg_resources
from rraster.Raster import Raster, RasterStack

# Find path to data within the package
pth = Path(pkg_resources.resource_filename('rraster', 'data'))

# Read in rasters
r1 = Raster(pth / '19990601_pr.tif')
r1 = Raster(pth / 'template.tif')

# Reproject with one line of code
reproj = r1.reproject(r2, method='bilinear')

# Do some stuff to the new raster
reproj = reproj * r2 # Possible because the two rasters now share the same projection and resolution.
reproj /= 18

# Make a RasterStack 
stk = RasterStack(reproj, r2)
# Calculate the pixel-wise mean
new_raster = stk.reduce(np.mean, axis=0)

# Save the new raster to disk.
new_raster.write('./example.tif')
```