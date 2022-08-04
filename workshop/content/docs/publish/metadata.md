---
title: Metadata
---

# Metadata

[OGC API Records](https://ogcapi.ogc.org/records/) provides access to repositories of metadata records. 
The API definition is likely to be adopted by OGC soon. pygeoapi contains an early implementation of the standard. 


## Publish a set of metadata records in pygeoapi

To use OGC API Record we will use Elasticsearch as data provider.

We will use this *workshop/docker/data/records/records.geojson*

To add data to elasticsearch instance we can use the command `docker exec elastic /add_data.sh ./data/records/records.geojson id`

Then we can uncomment the section related to example_catalog dataset in the `docker.config.yml`

## Client Access

QGIS supports OGC API Records via the [Metasearch plugin](https://docs.qgis.org/latest/en/docs/user_manual/plugins/core_plugins/plugins_metasearch.html). Metasearch originally focused on Catalogue Service for the Web (OGC:CSW) only, but has been extended to OGC API Records last year. Metasearch is a default plugin in QGIS (no installation required).

!!! question "Query OGC API Records from QGIS"

    Follow these steps to connect to a service and query datasets:

    - Locate the metasearch plugin in the web menu or on a toolbar (globe + binoculars). The main search panel opens with some popular catalogues pre-populated.

    ![Prepopulated catalogues](img/prepopulated-catalogues.png){ width=50% }

    - Open the `services` tab, to find the `new` button to create a new connection.
    - Add a connection to `https://demo.pygeoapi.io/master`.
    - Click `Serviceinfo` to get information about the service.
    - Return to the Search tab.
    - Select the connection you have just created.
    - type a search term and click `search`.
    - Notice that when you select a search result, a red box is drawn on the map highlighting the location of the dataset.

    ![Search results](img/search-results.png){ width=50% }

!!! note

    In optimal cases Metasearch will locate a file or service link on the metadata and enable the `add data` button (like it does for CSW). Unfortunately the OGC group is still discussing how to implement this in the standard. OGC API Records is not final yet, so its functionality may be subject to change.
