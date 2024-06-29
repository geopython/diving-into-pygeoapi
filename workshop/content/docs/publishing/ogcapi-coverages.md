---
title: Exercise 3 - Raster data via OGC API - Coverages
---

# Exercise 3 - Raster data via OGC API - Coverages

[OGC API - Coverages](https://ogcapi.ogc.org/coverages) provides a Web API to access raster
data (grids, remote sensing data, multidimensional data cubes):

* [OGC API - Coverages](https://docs.ogc.org/DRAFTS/19-087.html) (**draft**)

## pygeoapi support

pygeoapi supports the OGC API - Coverages draft specification, with [rasterio](https://rasterio.readthedocs.io) and [xarray](https://docs.xarray.dev) as core backends
as well as [CoverageJSON](https://covjson.org) and native output.

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-coverages.html) for more information on supported raster backends


## Publish a raster dataset

In the previous exercises we have demonstrated the steps involved to publish vector data and update the pygeoapi configuration. In this section we are going to publish a raster file in GeoTIFF format, from a [rasterio](https://rasterio.readthedocs.io) source provider.


!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor. Add a new dataset section as follows:

    ``` {.yaml linenums="1"}
    tartu-ntl:
        type: collection
        title: Night Time Light Data 
        description: Night Time Light Data averaged for 2023 in Tartu region.
        keywords:
            - Night Time Light
        links:
            -   type: text/html
                rel: canonical
                title: Nasa's Black Marble
                href: https://blackmarble.gsfc.nasa.gov/
                hreflang: it
        extents:
            spatial:
                bbox: [26.6264,58.32569,26.82632,58.433989]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            -   type: coverage
                name: rasterio
                data: /data/tartu/estonia_light.tif # place correct path here
                format:
                    name: GTiff
                    mimetype: application/tiff
    ```

!!! tip

    The rasterio provider `format.name` directive **requires** a valid [GDAL raster driver short name](https://gdal.org/drivers/raster/index.html)

Save the configuration and restart Docker Compose. Navigate to <http://localhost:5000/collections> to evaluate whether the new dataset has been published.

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

    If you do not have Python installed, consider running this exercise in a Docker container. See the [Setup Chapter](../setup.md#using-docker-for-python-clients). 

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
    >>> schema = w.collection_schema('gdps-temperature')
    >>> len(schema['field'])
    1
    >>> schema['properties']['1']['title']
    'Temperature [C]'
    >>> schema['properties']['1']['x-ogc-unit']
    '[C]'
    >>> schema['properties']['1']['type']
    'number'
    ```
    </div>

!!! note

    See the official [OWSLib documentation](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) for more examples.

# Summary

Congratulations! You are now able to publish raster data to pygeoapi.
