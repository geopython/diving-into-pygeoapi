---
title: Raster data
---

# Raster Data

## Client Access

OGC API Coverages is still under development at OGC. The GDAL team however already implemented an experimental plugin to interact with OGC API Coverages (maps, tiles and processes).

!!! question "Use GDAL to interact with OGC API Coverages"

    - Verify you have a recent GDAL installed, else use GDAL from OSGEO Live.
    - Run GDALINFO on command line to verify a connection to OGC API Coverages:

    ```
    gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
    ```
