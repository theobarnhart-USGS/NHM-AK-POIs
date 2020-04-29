---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.4.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import pandas as pd
import geopandas as gpd
import matplotlib.pyplot as plt
import numpy as np
from shapely.geometry import Point
```

```python
tmp1 = pd.read_excel('../data/NNA_sensor_lat_long.xlsx',sheet_name='YRITWC')
tmp2 = pd.read_excel('../data/NNA_sensor_lat_long.xlsx',sheet_name='USGS')

dat = pd.concat([tmp1,tmp2])
```

```python
geometries = [Point(xy) for xy in zip(dat.Longitude,dat.Latitute)]
dat = gpd.GeoDataFrame(dat, crs = {'init':'epsg:4326'}, geometry = geometries)
dat.to_file('../data/NNA_sensors.geojson', driver = 'GeoJSON')
```

```python
dat.plot()
```

```python

```
