---
title: pygeoapi as a Bridge to Other Services
---

# pygeoapi as a Bridge to Other Services

In this section we explore how pygeoapi can be used as a facade, or a bridge, to re-publish web services with different interfaces. These bridges can help [organisations migrating from OWS to OGC API](https://ogcapi-workshop.ogc.org/transition-and-migration/).

## Publishing WFS as OGC API - Features

A powerful use case for pygeoapi is to provide an OGC API - Features interface over existing Web Feature Service (WFS) 
or ESRI FeatureServer endpoints. In this scenario, you lower the barrier and increase the usability of existing services to 
a wider audience. Let's set up an API on top of an existing WFS hosted by the city of Florence.

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration in a text editor. 
    Find the line: 
    "# START - EXERCISE 2 - Proxy" 

    Add a new dataset section by uncommenting the lines up to
    "# END - EXERCISE 2 - Proxy":


    ``` {.yaml linenums="1"}
    suol_epicentri_storici:
        type: collection
        title: Epicenters of the main historical earthquakes
        description: Location of the epicenters of the main historical earthquakes in the territory of the Metropolitan City of Florence classified by year and intensity
        keywords:
            - earthquakes
        links:
            - type: text/xml
              rel: canonical
              title: Epicenters of the main historical earthquakes
              href: http://pubblicazioni.cittametropolitana.fi.it/geoserver/territorio/wfs?request=getCapabilities&service=WFS&version=2.0.0
              hreflang: it
        extents:
            spatial:
                bbox: [10.94, 43.52, 11.65, 44.17]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
              name: OGR
              data:
                  source_type: WFS
                  source: WFS:http://pubblicazioni.cittametropolitana.fi.it/geoserver/territorio/wfs?
                  source_capabilities:
                      paging: True
                  source_options:
                      OGR_WFS_LOAD_MULTIPLE_LAYER_DEFN: NO
                  gdal_ogr_options:
                      EMPTY_AS_NULL: NO
                      GDAL_CACHEMAX: 64
                      CPL_DEBUG: NO
              id_field: cpti_id
              crs:
                - http://www.opengis.net/def/crs/OGC/1.3/CRS84
                - http://www.opengis.net/def/crs/EPSG/0/4258
                - http://www.opengis.net/def/crs/EPSG/0/3857
                - http://www.opengis.net/def/crs/EPSG/0/3003
              storage_crs: http://www.opengis.net/def/crs/EPSG/0/3003
              title_field: d
              layer: territorio:suol_epicentri_storici
    ```

Save the file and restart Docker Compose. Navigate to `http://localhost:5000/collections` 
to evaluate whether the new dataset has been published.
 
Note these important configuration slices under `providers`:

* We use the pygeoapi [OGR Provider](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#ogr). 
This is the most versatile backend of pygeoapi for supporting numerous formats. Using the GDAL/OGR library (Python bindings) allows pygeoapi to connect to [around 80+ Vector Formats](https://gdal.org/drivers/vector).
We could have used the `OGR` Provider instead of the `SQLiteGPKG` Provider above in the `osm_places-vec` exercise above.

* `storage_crs` denotes the CRS (Coordinate Reference System) in which the dataset is stored (default is CRS84, i.e. 'longitude, latitude') 
* `crs` is an array of CRSs that can be specified for the Features to be returned (`crs=` parameter), or for their bounding box (`bbox-crs=` parameter). Default is also CRS84.
 
CRS support effectively allows pygeoapi to *reproject* the data from its storage CRS (here EPSG:3003)
according to [OGC API - Features - Part 2: Coordinate Reference Systems by Reference](https://docs.opengeospatial.org/is/18-058r1/18-058r1.html).
The Advanced section of this workshop will further [elaborate pygeoapi CRS support](../advanced/crs.md).


## Publishing WMS as OGC API - Maps

We can use the pygeoapi's WMSFacade provider as a bridge to serve an OGC WMS via OGC API - Maps.

We can use the MapServer demo server at: <https://demo.mapserver.org/cgi-bin/msautotest>

!!! note

    Feel free to use a WMS of your choice as you wish!

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor.

    Find the line: "# START - EXERCISE 5 - Maps".

    Uncomment or paste the configuration snippet below until the line that reads "## END - EXERCISE 5 - Maps". Be sure to keep the proper YAML indentation.

    ``` {.yaml linenums="1"}
    wms-facade-demo:
        type: collection
        title: WMS Facade demo
        description: WMS Facade demo
        keywords:
            - WMS facade
        links:
            - type: text/html
              rel: canonical
              title: MapServer
              href: https://mapserver.org
              hreflang: en
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: map
              name: WMSFacade
              data: https://demo.mapserver.org/cgi-bin/msautotest
              options:
                  layer: world_latlong
                  style: default
              format:
                  name: png
                  mimetype: image/png
    ```

Run the following requests in your web browser:

- default map: [http://localhost:5000/collections/wms-facade-demo/map?f=png](http://localhost:5000/collections/wms-facade-demo/map?f=png)
- specific width/height: [http://localhost:5000/collections/wms-facade-demo/map?f=png&width=800&height=600](http://localhost:5000/collections/wms-facade-demo/map?f=png&width=800&height=600)
- specific area of interest (bbox of Canada): [http://localhost:5000/collections/wms-facade-demo/map?f=png&width=800&height=600](http://localhost:5000/collections/wms-facade-demo/map?f=png&bbox=-142,42,-52,84)

!!! tip

    Try with your own bbox and width/height values!

## Publishing CSW as OGC API - Records

In this section we'll have a look at how to Publish a CSW data as an OGC API - Records, which then can be consumed by various clients.


!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor.
    Find the line: 
    "#START EXERCISE 7 - Environmental data via OGC - Environmental Data Retrieval" 

    Add a new dataset section by uncommenting the lines up of
    "# cite_demo configuration":    

    ``` {.yaml linenums="1"}
    cite_demo:
        type: collection
        title: pycsw OGC CITE demo and Reference Implementation
        description: pycsw is an OARec and OGC CSW server implementation written in Python. pycsw fully implements the OGC API - Records and OpenGIS Catalogue Service Implementation Specification (Catalogue Service for the Web). Initial development started in 2010 (more formally announced in 2011). The project is certified OGC Compliant, and is an OGC Reference Implementation. Since 2015, pycsw is an official OSGeo Project. pycsw allows for the publishing and discovery of geospatial metadata via numerous APIs (CSW 2/CSW 3, OpenSearch, OAI-PMH, SRU). Existing repositories of geospatial metadata can also be exposed, providing a standards-based metadata and catalogue component of spatial data infrastructures. pycsw is Open Source, released under an MIT license, and runs on all major platforms (Windows, Linux, Mac OS X)
        keywords:
            - ogc
            - cite
            - compliance
            - interoperability
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: record
            name: CSWFacade
            data: https://demo.pycsw.org/cite/csw
            id_field: identifier
            time_field: datetime
            title_field: title
    ```

## Publishing SensorThings API as OGC API - Features

In this section we'll have a look at how to Publish a SensorThings API as an OGC API - Features, which then can be consumed by various clients.
PygeoAPI allows to consume `Thing` , `Sensor` and `ObservedProperty`.

!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor.
    Find the line: 
    "#START EXERCISE 7 - Environmental data via OGC - Environmental Data Retrieval" 

    Add a new dataset section by uncommenting the lines up of
    "# toronto_bikes configuration": 

    ``` {.yaml linenums="1"}
    toronto_bikes:
        type: collection
        title: Toronto Bikes SensorThings
        description: The geographic location with coordinates for the Toronto bike share station
        keywords:
            - sediments
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
                name: SensorThings
                data: https://toronto-bike-snapshot.sensorup.com/v1.0/
                entity: Things
    ```