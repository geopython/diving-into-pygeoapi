---
title: Raster data
---

# Raster Data

Access to coverage datasets (grids) is managed through the 
[OGC Coverage API](https://ogcapi.ogc.org/coverages/). The API is still under development at the OGC, but pygeoapi contains an early implementation of OGC API Coverages.

## Publish a raster dataset

Download and unzip the tiff file [53.tif](http://dati.cittametropolitana.fi.it/geonetwork/srv/api/records/cmfi:419774cb-e812-4ca4-991d-97f0b747e017/attachments/53.zip). Add it to pygeoapi.

## Client Access

OGC API Coverages is still under development at OGC. The GDAL team however already implemented an experimental plugin to interact with OGC API Coverages (maps, tiles and processes).

!!! question "Use GDAL to interact with OGC API Coverages"

    - Verify you have a recent GDAL installed, else use GDAL from OSGEO Live.
    - Run GDALINFO on command line to verify a connection to OGC API Coverages:

    ```
    gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
    ```
