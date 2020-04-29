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
names = ['NWSID','USGSID','GOESID','NWSHSA','LAT','LON','DESC']
dat = pd.read_csv('../data/AK_USGS-HADS_SITES.txt', sep='|', skiprows=4, header=None, names=names)
```

```python
dat['lat_deg'] = dat.LAT.map(lambda x: float(x.split(' ')[0]))
dat['lat_min'] = dat.LAT.map(lambda x: float(x.split(' ')[1]))
dat['lat_sec'] = dat.LAT.map(lambda x: float(x.split(' ')[2]))

dat['lon_deg'] = dat.LON.map(lambda x: float(x.split(' ')[0]))
dat['lon_min'] = dat.LON.map(lambda x: float(x.split(' ')[1]))
dat['lon_sec'] = dat.LON.map(lambda x: float(x.split(' ')[2]))

dat['LATdd'] = dat.lat_deg + ((dat.lat_min + (dat.lat_sec / 60.)) / 60.)
dat['LONdd'] = (dat.lon_deg + ((dat.lon_min + (dat.lon_sec / 60.)) / 60.)) * -1
```

```python
dat.head()
```

```python
geometries = [Point(xy) for xy in zip(dat.LONdd,dat.LATdd)]
crs = {'init':'epsg:4326'}
dat = gpd.GeoDataFrame(dat, crs = crs, geometry = geometries)

dat.drop(['LAT','LON','lat_deg','lat_min','lat_sec','lon_deg','lon_min','lon_sec','LATdd','LONdd'], axis=1, inplace=True)
dat.to_file('../data/nws_pois.geojson', driver = 'GeoJSON')
```

```python
dat.plot()
```
