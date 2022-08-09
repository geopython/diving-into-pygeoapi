---
title: Raster data
---

# Raster Data

Access to coverage datasets (grids) is managed through the 
[OGC Coverage API](https://ogcapi.ogc.org/coverages/). The API is still under development at the OGC, but pygeoapi contains an early implementation of OGC API Coverages.

## Publish a raster dataset

In the previous section you have seen in general which steps are involved to change the pygeoapi configuration file to load a dataset. In this section we are going to publish a GeoTiff raster file, from a [rasterio](https://rasterio.readthedocs.io) source.

Download and unzip a GeoTiff file, eg. [53.tif](http://dati.cittametropolitana.fi.it/geonetwork/srv/api/records/cmfi:419774cb-e812-4ca4-991d-97f0b747e017/attachments/53.zip).

You are going to add a file `53_ED1_G.tif` to pygeoapi which is available in the workshop data folder.

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor. Add a new dataset section, defined by:

    ``` {.yaml linenums="1"}
    firenze-terrains:
        type: collection
        title: Administrative boundaries before 2014
        description: Cadastral parcels (terrains) from the cadastre. Territory Agency; SIT and Information Networks;
        keywords:
            -   Cadastral parcels
        links:
            -   type: text/html
                rel: canonical
                title: Administrative boundaries before 2014
                href: http://dati.cittametropolitana.fi.it/geonetwork/srv/metadata/cmfi:419774cb-e812-4ca4-991d-97f0b747e017
                hreflang: it
        extents:
            spatial:
                bbox: [10.70,43.43,11.76,44.25]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            -   type: coverage
                name: rasterio
                data: /data/53_ED1_G.tif # place correct path here
                format:
                    name: GTiff
                    mimetype: application/tiff
    ```

!!! tip "The rasterio provider `format.name` directive **requires** a valid [GDAL raster driver short name](https://gdal.org/drivers/raster/index.html)"

Save the file and restart the docker compose. Navigate to `http://localhost:5000/collections` to evaluate if the new dataset is available.


## Client Access

OGC API Coverages is still under development at OGC. The GDAL team however already implemented an experimental plugin to interact with OGC API Coverages (maps, tiles and processes).

!!! question "Use GDAL to interact with OGC API Coverages"

    - Verify you have a recent GDAL installed, else use GDAL from OSGeoLive.
    - Run GDALINFO on command line to verify a connection to OGC API Coverages:

    ```
    gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
    ```
