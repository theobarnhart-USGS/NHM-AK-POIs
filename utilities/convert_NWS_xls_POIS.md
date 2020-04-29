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
names = ['NWSID','startDate','endDate','lat','lon','name','Notes']
dat = pd.read_excel('../data/Metadata_for_PRMS_group_Feb2020.xlsx', skiprows=2, header = None,
                    names = names, sheet_name='Merged-Use-This', dtype = {'startDate':str,
                                                                            'endDate':str})
```

```python
dat.dtypes
```

```python
geometries = [Point(xy) for xy in zip(dat.lon,dat.lat)]
crs = {'init':'epsg:4326'}
dat = gpd.GeoDataFrame(dat, geometry = geometries, crs = crs)
dat.to_file('../data/nws_xls_pois.geojson',driver='GeoJSON')
```

```python
dat.plot()
```

```python
dat.to_crs(epsg=3338).plot()
```

```python

```
