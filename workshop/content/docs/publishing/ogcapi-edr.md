---
title: Exercise 7 - Environmental data via OGC - Environmental Data Retrieval
---

# Exercise 7 - Environmental data via OGC - Environmental Data Retrieval

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

Save the configuration and restart docker compose. Navigate to `http://localhost:5000/collections` to evaluate whether the new dataset has been published.

At first glance, the `icoads-sst` collection appears as a normal OGC API - Coverages collection. Let's look a bit closer at the colleciton description:

# Client access

Currently there is no support for EDR in common tooling. The example below provides a generic workflow using the [Python requests library](https://requests.readthedocs.io):

<div class="termy">
```python
>>> import requests
>>> collection = requests.get('http://localhost:5000/collections/icoads-sst').json()
>>> collection['id']
'icoads-sst'
>>> collection['title']
'International Comprehensive Ocean-Atmosphere Data Set (ICOADS)'
>>> collection['description']
'International Comprehensive Ocean-Atmosphere Data Set (ICOADS)'
>>> collection['parameter-names'].keys()
dict_keys(['SST', 'AIRT', 'UWND', 'VWND'])
>>> params = {'coords': 'POINT(-28 14)', 'parameter-name': 'SST'}
>>> position_query = requests.get('http://localhost:5000/collections/icoads-sst/position', params=params).json()
>>> position_query['ranges']['SST']['values']
[26.755414962768555, 26.303892135620117, 26.512916564941406, 26.799564361572266, 27.48826026916504, 28.04759979248047, 28.745832443237305, 28.5635986328125, 28.272104263305664, 28.526521682739258, 28.25160026550293, 27.074399948120117]
```
</div>

# Summary

Congratulations!  You are now able to publish environmental data to pygeoapi.
