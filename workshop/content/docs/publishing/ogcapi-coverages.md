---
title: Exercise 3 - Raster data via OGC API - Coverages
---

# Exercise 3 - Raster data via OGC API - Coverages

[OGC API - Coverages](https://ogcapi.ogc.org/coverages) provides a Web API to access raster
data (grids, remote sensing data, multidimensional data cubes):

* [OGC API - Coverages](https://ogcapi.ogc.org/coverages/) (**draft**)

## pygeoapi support

pygeoapi supports the OGC API - Coverages draft specification, with [rasterio](https://rasterio.readthedocs.io) and [xarray](https://docs.xarray.dev) as core backends
and [CoverageJSON](https://covjson.org) and native output.

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-coverages.html) for more information on supported raster backends


## Publish a raster dataset

In the previous exercises we have demonstrated the steps involved to publish vector data and update the pygeoapi configuration. In this section we are going to
publish a raster file in GeoTIFF format, from a [rasterio](https://rasterio.readthedocs.io) source provider.

Download and unzip the GeoTIFF file:

<div class="termy">
```bash
cd workshop/exercises/data
curl -O http://dati.cittametropolitana.fi.it/geonetwork/srv/api/records/cmfi:419774cb-e812-4ca4-991d-97f0b747e017/attachments/53.zip
unzip 53.zip
```
</div>

You can now add `53_ED1_G.tif` to pygeoapi:

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor. Add a new dataset section as follows:

    ``` {.yaml linenums="1"}
    firenze-terrains:
        type: collection
        title: Administrative boundaries before 2014
        description: Cadastral parcels (terrains) from the cadastre. Territory Agency; SIT and Information Networks;
        keywords:
            - Cadastral parcels
        links:
            - type: text/html
              rel: canonical
              title: Administrative boundaries before 2014
              href: http://dati.cittametropolitana.fi.it/geonetwork/srv/metadata/cmfi:419774cb-e812-4ca4-991d-97f0b747e017
              hreflang: it
        extents:
            spatial:
                bbox: [10.70,43.43,11.76,44.25]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: coverage
              name: rasterio
              data: /data/53_ED1_G.tif # place correct path here
              format:
                  name: GTiff
                  mimetype: application/tiff
    ```

!!! tip

    The rasterio provider `format.name` directive **requires** a valid [GDAL raster driver short name](https://gdal.org/drivers/raster/index.html)

Save the configuration and restart docker compose. Navigate to `http://localhost:5000/collections` to evaluate whether the new dataset has been published.

## Client access

### GDAL/OGR

[GDAL/OGR](https://gdal.org) provides support for [OGC API - Coverages](https://gdal.org/drivers/raster/ogcapi.html). This means you can use `gdalinfo` to query and convert data from OGC API - Coverages endpoints just like any other raster data source.  This also means you can make connections to OGC API - Coverages endpoints from any software which has an interface to GDAL, such as MapServer, GeoServer, Manifold, FME, ArcGIS, etc.


!!! question "Use GDAL to interact with OGC API - Coverages"

    - Verify you have a recent GDAL installed, else use GDAL from OSGeoLive
    - Run `gdalinfo` on the command line to verify a connection to OGC API - Coverages:

    <div class="termy">
    ```
    gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
    ```
    </div>

### OWSLib

[OWSLib](https://owslib.readthedocs.io) is a Python library to interact with OGC Web Services and supports a number of OGC APIs including OGC API - Coverages.

!!! question "Interact with OGC API - Coverages via OWSLib"

    If you do not have Python installed, consider running this exercise in a Docker container or in a cloud environment. 

    <div class="termy">
    ```bash
    pip3 install owslib
    ``` 
    </div>

    <div class="termy">
    ```python
    >>> from owslib.ogcapi.coverages import Coverages
    >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
    >>> w = Coverages(SERVICE_URL)
    >>> w.url
    'https://demo.pygeoapi.io/master/'
    >>> gdps = w.collection('gdps-temperature')
    >>> gdps['id']
    'gdps-temperature'
    >>> gdps['title']
    'Global Deterministic Prediction System sample'
    >>> gdps['description']
    'Global Deterministic Prediction System sample'
    >>> domainset = w.coverage_domainset('gdps-temperature')
    >>> domainset['generalGrid']['axisLabels']
    ['Long', 'Lat']
    >>> domainset['generalGrid']['gridLimits']['axisLabels']
    ['i', 'j']
    >>> rangetype = w.coverage_rangetype('gdps-temperature')
    >>> len(rangetype['field'])
    1
    >>> rangetype['field'][0]['name']
    'Temperature [C]'
    >>> rangetype['field'][0]['uom']['code']
    '[C]'
    >>> rangetype['field'][0]['encodingInfo']['dataType']
    'http://www.opengis.net/def/dataType/OGC/0/float64'
    ```
    </div>

!!! note

    See the official [OWSLib documentation](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) for more examples.

# Summary

Congratulations! You are now able to publish raster data to pygeoapi.
