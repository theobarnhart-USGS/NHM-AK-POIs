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

nws2 = nws2.drop_duplicates('NWSID')
nws1 = nws1.drop_duplicates('NWSID')
```

```python
nws1.head()
```

```python
nws1.columns = ['stationID_NWS', 'stationID', 'GOESID', 'NWSHSA', 'DESC', 'geometry']
nws1.drop(['GOESID', 'NWSHSA', 'DESC'], axis=1, inplace=True)
```

```python
nws2.columns = ['stationID_NWS','startDate', 'endDate', 'lat', 'lon', 'name', 'Notes','geometry']
nws2.drop(['startDate', 'endDate', 'lat', 'lon', 'name', 'Notes'], axis=1, inplace=True)
```

```python
nws = pd.concat([nws1,nws2])
```

```python
nws.shape
```

```python
nws.drop_duplicates('stationID_NWS', inplace=True)
```

```python
nna.columns = ['riverName','community','Latitute', 'Longitude','stationID', 'geometry']
nna.drop(['Latitute', 'Longitude'], axis=1, inplace=True)
```

```python
wbeep.columns = ['stationID','stationName', 'status', 'AltStationID', 'geometry']
wbeep.drop(['stationName', 'status', 'AltStationID'], axis=1, inplace=True)
```

```python
usgs = pd.concat([nna,wbeep])
```

```python
usgs.stationID.fillna('NaN', inplace=True)
nws.stationID.fillna('NaN', inplace=True)
```

```python
noID = usgs.loc[usgs.stationID == 'NaN'].copy()
```

```python
withID = usgs.loc[usgs.stationID != 'NaN'].copy()
```

```python
withID.drop_duplicates('stationID', inplace=True)
```

```python
usgs = pd.concat([noID,withID])
```

```python
pois = pd.concat([usgs,nws])
```

```python
noID = pois.loc[pois.stationID == 'NaN'].copy()
withID = pois.loc[pois.stationID != 'NaN'].copy()
```

```python
withID.shape
```

```python
withID.drop_duplicates('stationID').shape
```

```python
nws.head()
```

```python
nws.tail()
```

```python
usgs.drop_duplicates('stationID')
```

```python
usgs
```

```python
pois2 = pd.concat([nws1,nws2,nna,wbeep])
```

```python
pois2.shape
```

```python
pois.plot()
```

```python
sws = gpd.read_file('../../WBEEP_AK/data/gis/AKhydroStudyBasins.shp')
```

```python
swsP = gpd.read_file('../../WBEEP_AK/data/gis/sir20165024_geodatabase_BasinChar.gdb/SIR_2016_5024_BasinChar.gdb',layer = 'SIR_2016_5024_BasinChar_Alb')
```

```python
for col in ['Station_name', 'Notes', 'LAT', 'LON', 'LAT_CENT',
       'LONG_CENT', 'DRNAREA', 'PRECPRIS00', 'JANMINTMP', 'I24H2Y', 'ELEV',
       'LAKEAREA', 'GLACIER', 'LC01WETLND', 'LC01FOREST', 'LC01SNOIC',
       'LC01SCRUB', 'LC01BARE', 'PERMAFROST_LOWUP', 'PERMAFROST_MTN',
       'BROADLEAF', 'NEEDLELEAF', 'MIXBROADNEEDLE']:
    del swsP[col]
```

```python
swsP.columns = ['stationID','geometry']
```

```python
swsP = swsP.to_crs(epsg=4326)
```

```python
pois = pd.concat([pois,swsP])
```

```python
noID = pois.loc[pois.stationID == 'NaN'].copy()
withID = pois.loc[pois.stationID != 'NaN'].copy()
```

```python
withID.drop_duplicates('stationID',inplace = True)
```

```python
pois = pd.concat([noID, withID])
```

```python
pois.plot()
```

```python
pois.to_file('../merged_pois.geojson', driver = 'GeoJSON')
pois.to_crs(epsg=3338).to_file('../merged_pois.shp')
```

```python
pois = pois[['stationID','stationID_NWS','geometry']]
```

```python
# function to merge id values
def merge_stationIDs(df):
    if df.stationID != 'NaN':
        return df.stationID
    elif df.stationID == 'NaN' and df.stationID_NWS != 'NaN':
        return df.stationID_NWS
    else:
        return 'NaN'
```

```python
pois['mergedID'] = pois.apply(merge_stationIDs,axis = 1)
```

```python
pois = pois[['mergedID','geometry']]
pois.columns = ['stationID','geometry']
```

```python

```
