---
title: Exercise 5 - Metadata via OGC API - Records
---

# Exercise 5 - Metadata via OGC API - Records

[OGC API - Records](https://ogcapi.ogc.org/records) provides a Web API with the capability to create, modify,
and query metadata on the Web:

* [OGC API - Records: Part 1: Core](https://docs.ogc.org/DRAFTS/20-004.html) (**draft**)

OGC API - Records uses OGC API - Features as a building block, thus enabling streamlined deployment and integration
for clients and users.

## pygeoapi support

pygeoapi supports the OGC API - Records draft specification, using Elasticsearch and TinyDB [rasterio](https://rasterio.readthedocs.io) as core backends.

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-records.html) for more information on supported catalogue/metadata backends


## Publish metadata records in pygeoapi

With pygeoapi we can setup OGC API - Records using any supported data provider. In this exercise we will use the [TinyDB](https://tinydb.readthedocs.io/en/latest/index.html)
Catalogue backend. We will use the sample catalogue in `workshop/exercises/data/records/catalogue.tinydb`.

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor. Add a new dataset section as follows:

``` {.yaml linenums="1"}
    example_catalogue:
        type: collection
        title: FOSS4G Florence Record catalogue
        description: FOSS4G Florence Record catalogue (OGC API - Records)
        keywords:
            - Services
            - Infrastructures
            - Florence
            - FOSS4G
        links:
            - type: text/html
              rel: canonical
              title: information
              href: http://opendata.comune.firenze.it
              hreflang: en-US
        extents:
            spatial:
                bbox: [11.145, 43.718, 11.348, 43.84]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: record
              name: TinyDBCatalogue
              data: /data/records/catalogue.tinydb
              id_field: externalId
              time_field: recordCreated
              title_field: title
```

Save the configuration and restart docker compose. Navigate to `http://localhost:5000/collections` to evaluate whether the new dataset has been published.

## Metadata formats

By default, pygeoapi supports and expects the OGC API - Records core record model and queryables. For additional metadata formats, you can
develop your own custom pygeoapi plugin, or convert your metadata to OGC API - Records core record model before adding to pygeoapi.

### Sample ISO 19139 to TinyDBCatalogue loader

It's possible to load more example ISO19139 metadata in a TinyDB database with [the following script](https://github.com/geopython/pygeoapi/blob/master/tests/load_tinydb_records.py) ([raw](https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py))


<div class="termy">
```bash
python3 load_tinydb_records.py /xml_folder/ /db_folder/sample-records.tinydb
```
</div>

## Client access

### QGIS

QGIS supports OGC API - Records via the [MetaSearch plugin](https://docs.qgis.org/latest/en/docs/user_manual/plugins/core_plugins/plugins_metasearch.html). MetaSearch originally focused on Catalogue Service for the Web (OGC:CSW) only, but has been extended to OGC API - Records. MetaSearch is a default plugin in QGIS and requires no further installation.

!!! question "Query OGC API - Records from QGIS"

    Follow these steps to connect to a service and query datasets:

    - Locate the MetaSearch plugin in the Web menu or on the Toolbar ![MetaSearch icon](https://docs.qgis.org/latest/en/_images/MetaSearch.png "MetaSearch icon"). The main search panel will appear with the default MetaSearch catalogue list already populated.

    ![Pre-populated catalogues](../assets/images/prepopulated-catalogues.png){ width=50% }

    - open the `Services` tab, to find the `New` button to create a new connection
    - add a connection to `https://demo.pygeoapi.io/master`
    - click `Service Info` to get information about the service
    - return to the Search tab
    - select the connection you have just created
    - type a search term and click `search`
    - notice that when you select a search result, a red footprint is drawn on the map highlighting the location of the dataset

    ![Search results](../assets/images/search-results.png){ width=50% }

[OWSlib](https://geopython.github.io/OWSLib) is a Python library to interact with OGC Web Services and supports a number of OGC APIs including OGC API - Records.

!!! question "Interact with OGC API - Records via OWSLib"

    If you do not have Python installed, consider running this exercise in a Docker container or in a cloud environment.

    <div class="termy">
    ```bash
    pip3 install owslib
    ``` 
    </div>

    Then start a Python console session with: `python` (stop the session by typing `exit()`).

    <div class="termy">
    ```python
    >>> from owslib.ogcapi.records import Records
    >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
    >>> w = Records(SERVICE_URL)
    >>> w.url
    'https://demo.pygeoapi.io/master'
    >>> dutch_metacat = w.collection('dutch-metadata')
    >>> dutch_metacat['id']
    'dutch-metadata'
    >>> dutch_metacat['title']
    'Sample metadata records from Dutch Nationaal georegister'
    >>> dutch_metacat['description']
    'Sample metadata records from Dutch Nationaal georegister'
    >>> dutch_metacat_query = w.collection_items('dutch-metadata', limit=1)
    >>> dutch_metacat_query['numberMatched']
    198
    >>> dutch_metacat_query['numberReturned']
    1
    >>> dutch_metacat_query = w.collection_items('dutch-metadata', q='Wegpanorama')
    >>> dutch_metacat_query['numberMatched']
    2
    ```
    </div>

!!! note

    See the official [OWSLib documentation](https://geopython.github.io/OWSLib/usage.html#ogc-api) for more examples.


# Summary

Congratulations!  You are now able to publish metadata to pygeoapi.
