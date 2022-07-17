---
title: Metadata
---

# Metadata


## Client Access

QGIS supports OGC API Records via the [Metasearch plugin](https://docs.qgis.org/latest/en/docs/user_manual/plugins/core_plugins/plugins_metasearch.html). Metasearch originally focussed on Web Catalgue Service (OGC:WCS) only, but has been extended to OGC API Records last year. Metasearch is a default plugin in QGIS (no installation required).

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
