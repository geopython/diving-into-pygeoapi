---
title: Exercise 7 - Environmental data via OGC API - Environmental Data Retrieval
---

# Exercise 7 - Environmental data via OGC API - Environmental Data Retrieval

[OGC API - Environmental Data Retrieval](https://ogcapi.ogc.org/edr) provides a Web API to access
environmental data using well defined query patterns:

* [OGC API - Environmental Data Retrieval Standard](https://docs.ogc.org/is/19-086r4/19-086r4.html)

OGC API - Environmental Data Retrieval uses OGC API - Features as a building block, thus enabling
streamlined integration for clients and users.  EDR can be considered a convenience API which does
not require in depth knowledge about the underlying data store/model.

## pygeoapi support

pygeoapi supports the OGC API - Environmental Data Retrieval specification by leveraging both feature
and coverage provider plugins.

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-edr.html) for more information on supported EDR backends


## Publish environmental data in pygeoapi

Let's try publishing some ICOADS data via the EDR xarray plugin. The sample ICOADS data can be found in `workshop/exercises/data/coads_sst.nc`:


!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor. Add a new dataset section as follows:

``` {.yaml linenums="1"}
    icoads-sst:
        type: collection
        title: International Comprehensive Ocean-Atmosphere Data Set (ICOADS)
        description: International Comprehensive Ocean-Atmosphere Data Set (ICOADS)
        keywords:
            - icoads
            - sst
            - air temperature
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
            temporal:
                begin: 2000-01-16T06:00:00Z
                end: 2000-12-16T06:00:00Z
        links:
            - type: text/html
              rel: canonical
              title: information
              href: https://psl.noaa.gov/data/gridded/data.coads.1deg.html
              hreflang: en-US
        providers:
            - type: edr
              name: xarray-edr
              data: /data/coads_sst.nc
              format:
                  name: NetCDF
                  mimetype: application/x-netcdf
```

Save the configuration and restart Docker Compose. Navigate to <http://localhost:5000/collections> to evaluate whether the new dataset has been published.

At first glance, the `icoads-sst` collection appears as a normal OGC API - Coverages collection. Look a bit closer at the collection description, and notice
that there is a `parameter_names' key that describes EDR parameter names for the collection queries.

### OWSLib - Advanced

[OWSLib](https://owslib.readthedocs.io) is a Python library to interact with OGC Web Services and supports a number of OGC APIs including OGC API - Environmental Data Retrieval.

!!! question "Interact with OGC API - Environmental Data Retrieval via OWSLib"

    If you do not have Python installed, consider running this exercise in a Docker container. See the [Setup Chapter](../setup.md#using-docker-for-python-clients).

    === "Linux/Mac"

        <div class="termy">
        ```bash
        pip3 install owslib
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        pip3 install owslib
        ```
        </div>

    Then start a Python console session with `python3` (stop the session by typing `exit()`).

    === "Linux/Mac"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.edr import  EnvironmentalDataRetrieval
        >>> w = EnvironmentalDataRetrieval('https://demo.pygeoapi.io/master')
        >>> w.url
        'https://demo.pygeoapi.io/master'
        >>> api = w.api()  # OpenAPI document
        >>> collections = w.collections()
        >>> len(collections['collections'])
        13
        >>> icoads_sst = w.collection('icoads-sst')
        >>> icoads_sst['parameter-names'].keys()
        dict_keys(['SST', 'AIRT', 'UWND', 'VWND'])
        >>> data = w.query_data('icoads_sst', 'position', coords='POINT(-75 45)', parameter_names=['SST', 'AIRT'])
        >>> data  # CoverageJSON data
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.edr import  EnvironmentalDataRetrieval
        >>> w = EnvironmentalDataRetrieval('https://demo.pygeoapi.io/master')
        >>> w.url
        'https://demo.pygeoapi.io/master'
        >>> api = w.api()  # OpenAPI document
        >>> collections = w.collections()
        >>> len(collections['collections'])
        13
        >>> icoads_sst = w.collection('icoads-sst')
        >>> icoads_sst['parameter-names'].keys()
        dict_keys(['SST', 'AIRT', 'UWND', 'VWND'])
        >>> data = w.query_data('icoads_sst', 'position', coords='POINT(-75 45)', parameter_names=['SST', 'AIRT'])
        >>> data  # CoverageJSON data
        ```
        </div>

!!! note

    See the official [OWSLib documentation](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) for more examples.

# Summary

Congratulations!  You are now able to publish environmental data to pygeoapi.
