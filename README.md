# NHM-AK-POIs
Points of interest for the National Hydrologic Model Alaska Domain

Contact: Theodore Barnhart, tbarnhart@usgs.gov

## Purpose
The purpose of this repository is to facilitate collecting points of interest (POIs) for the U.S. Geological Survey National Hydrologic Model Domain. 

## Domain
The NHM Alaska Domain covers the state of Alaska and the watersheds draining to the state of Alaska. 

![NHM Alaska Domain](/img/ak_domain_gf_v1_1.png)

## Points of Interest
The following points of interest have already been gathered. Pour points from HUC12 watersheds will also be included as POIs; however, we have included them as a separate file because there are so many of them.

POI data sources:
- USGS streamgages in the domain
- Canadian Streamgages in the domain
- HUC12 pour points in the domain

## Instructions
Please download and view the POIs and HUC12s and add any points of interest you may have to the POI file. Forking this repository and then cloning to your machine is probably the easiest way to do this (see below). Then send us the updated POI file via a pull request (see below). 

Fields to include:
- StationID: Identifier for the station.
- StationName: Name of the station.
- status: Active / Inactive status.
- AltStationID: Identifier of other stations that are collocated with the StationID.

## Note on Projections
These data are distributed as WGS84 (`EPSG:4326`) for purposes of versioning the POI list using geoJSON. We'll convert everything to Alaska Albers Equal Area (`EPSG:3338`) for use in the project.

## Code Examples

### Convert geoJSON to Shapefile
For use with your favorite GIS of choice (if it cannot read a geoJSON).

```python
import geopandas as gpd
poiFL = 'ak_pois.geojson'
poiSHP = 'ak_pois.shp'
dat = gpd.read_file(poiFL, driver = 'GeoJSON') # read in the geoJSON
dat.to_file(poiSHP) # write out the shapefile
```
### Convert Shapefile to geoJSON
For committing changes to the POI list. Shapefiles are ignored in this repository

```python
dat = gpd.read_file(poiSHP) # read in the (modified) shapefile
dat.to_file(poiFL, driver = 'GeoJSON') # write out a new geoJSON for upload to git
```

### Fork this Repository
Fork this repository to make changes, then create a pull request so we can merge your changes back into the repository.
https://help.github.com/en/github/getting-started-with-github/fork-a-repo

### Pull Request Back to this Repository
Send us the changes you have made.
https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork