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
```

```python
nws1 = gpd.read_file('../data/nws_pois.geojson')
nws2 = gpd.read_file('../data/nws_xls_pois.geojson')
nna = gpd.read_file('../data/NNA_sensors.geojson')
wbeep = gpd.read_file('../ak_pois.geojson')
```

```python
nws1.columns = ['stationID_NWS', 'stationID_USGS', 'GOESID', 'NWSHSA', 'DESC', 'geometry']
nws1.drop(['GOESID', 'NWSHSA', 'DESC'], axis=1, inplace=True)
```

```python
nws2.columns = ['stationID_NWS','startDate', 'endDate', 'lat', 'lon', 'name', 'Notes','geometry']
nws2.drop(['startDate', 'endDate', 'lat', 'lon', 'name', 'Notes'], axis=1, inplace=True)
```

```python
nna.columns = ['riverName','community','Latitute', 'Longitude', 'geometry']
nna.drop(['Latitute', 'Longitude'], axis=1, inplace=True)
```

```python
wbeep.columns = ['stationID_USGS','stationName', 'status', 'AltStationID', 'geometry']
wbeep.drop(['stationName', 'status', 'AltStationID'], axis=1, inplace=True)
```

```python
pois = pd.concat([nws1,nws2,nna,wbeep])
```

```python
pois.plot()
```

```python
pois.to_file('../merged_pois.geojson', driver = 'GeoJSON')
```
