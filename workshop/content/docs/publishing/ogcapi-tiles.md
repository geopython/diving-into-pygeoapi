---
title: Exercise 4 - Tiles of geospatial data via OGC API - Tiles
---

# Exercise 4 - Tiles of geospatial data via OGC API - Tiles

[OGC API - Tiles](https://ogcapi.ogc.org/tiles) provides a Web API to deliver tiles of geospatial information. Different forms of geospatial information are supported, such as tiles of vector features ("vector tiles"), coverages, maps (or imagery) and potentially eventually additional types of tiles of geospatial information. The standard is available on this document:
 
* [OGC API - Tiles: Part 1: Core](https://docs.ogc.org/is/20-057/20-057.html)

!!! note
    OGC API - Tiles extends the `collections/*` URL structure (tilesets are listed under `/collections/example/tiles`:

    ```
    https://demo.pygeoapi.io/collections/lakes/tiles/WorldCRS84Quad/{tileMatrix}/{tileRow}/{tileCol}?f=mvt
    ```

## pygeoapi support

pygeoapi supports the core OGC API - Tiles specification, and is able to advertise an existing tileset. Note that pygeoapi
itself does not render tiles from source data. It supports publishing pre-rendered tiles from a static url or from a tile server with a `xyz` url template.

!!! note

    The OGC API - Tiles URL structure is compatible with XYZ layers in common libraries such as OpenLayers, Leaflet and MapML

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-tiles.html) for more information on supported tile backends

!!! note

    pygeoapi currently supports two well known Tile Matrix Sets: `WorldCRS84Quad` and `WebMercatorQuad`. Their definition is published on the [/TileMatrixSets](https://demo.pygeoapi.io/master/TileMatrixSets) end point.  

## Publish pre-rendered vector tiles

In this scenario, tiles must be pre-rendered before serving. Existing tools to create tiles
include, but are not limited to:

* [TileMill](https://tilemill-project.github.io/tilemill)
* [MapProxy](https://mapproxy.org)
* [QGIS](https://www.qgistutorials.com/en/docs/creating_basemaps_with_qtiles.html)
* [tippecanoe](https://github.com/mapbox/tippecanoe)

For this exercise, you will publish a vector dataset of the [bathing water sources in Estonia](https://avaandmed.eesti.ee/datasets/joogiveeallikad), from the location below:

* data: `workshop/exercises/data/tartu/bathingwater-estonia.geojson`

Let's generate the tiles as the first step using tippecanoe:

!!! example "Using tippecanoe to generate vector tiles"

    === "Linux/Mac"
    
        <div class="termy">
        ```bash
        cd workshop/exercises
        docker run -it --rm -v $(pwd)/data:/data emotionalcities/tippecanoe \
        tippecanoe -r1 -pk -pf --output-to-directory=/data/tiles/ --force --maximum-zoom=20 \
        --extend-zooms-if-still-dropping --no-tile-compression /data/tartu/bathingwater-estonia.geojson
        ```
        </div>
     
    === "Windows"
    
        <div class="termy">
        ```bash
        cd workshop/exercises
        docker run -it --rm -v ${pwd}/data:/data emotionalcities/tippecanoe \
        tippecanoe -r1 -pk -pf --output-to-directory=/data/tiles/ --force --maximum-zoom=20 \
        --extend-zooms-if-still-dropping --no-tile-compression /data/tartu/bathingwater-estonia.geojson
        ```
        </div>
 
!!! question "Update the pygeoapi configuration"

    Open the pygeoapi configuration in a text editor. Add a new dataset section as follows:

``` {.yaml linenums="1"}
    bathingwater-estonia:
        type: collection
        title: Bathing water sources
        description: Data of bathing water sources used by water supply systems under the supervision of the Health Board from the Water Health Information System.
        keywords:
          - Water
          - Water bodies
          - Drilled wells
          - Surface water
          - Groundwater
          - Environmental health
          - Health
          - Bathing water
        links:
            - type: text/html
              rel: canonical
              title: information
              href: https://avaandmed.eesti.ee/api/datasets/slug/supluskohad
              hreflang: en-US
        extents:
            spatial:
                bbox: [22.2290936066586440,57.6912449743385451,28.2024877654160555,59.6097269178904412]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
            temporal:
                begin: null
                end: null  # or empty
        providers:
            - type: feature
              name: GeoJSON
              data: /data/tartu/bathingwater-estonia.geojson
              id_field: id
            - type: tile
              name: MVT-tippecanoe
              data: /data/tiles/  # local directory tree
              options:
                zoom:
                    min: 0
                    max: 16
              format:
                    name: pbf
                    mimetype: application/vnd.mapbox-vector-tile
```

Save the file and restart Docker Compose. Navigate to <http://localhost:5000/collections> to evaluate whether the new dataset has been published.

Additional check for the following tile specific endpoints in the `bathingwater-estonia` collection:

- tile links in <http://localhost:5000/collections/bathingwater-estonia/tiles>
- tile metadata in <http://localhost:5000/collections/bathingwater-estonia/tiles/WebMercatorQuad/metadata>

![TileSet](../assets/images/vtiles-estonia.png)

## Publish vector tiles from Elasticsearch

Elasticsearch provides a middleware that [renders an index on the fly, as vector tiles](https://www.elastic.co/blog/introducing-elasticsearch-vector-tile-search-api-for-geospatial). This middleware is also supported by the pygeoapi mvt backend.

If you want to explore publishing vector tiles using Elasticsearch clone [pygeoapi-examples](https://github.com/geopython/pygeoapi-examples/) repository:

<div class="termy">
```bash
git checkout https://github.com/geopython/pygeoapi-examples.git
```
</div>

Then change into the `docker/mvt-elastic` folder:

<div class="termy">
```bash
cd docker/mvt-elastic
```
</div>

Edit the `add-data.sh` script on the `ES` folder, adding these two lines before the end:

``` {.yaml linenums="1"}

    curl -o /tmp/bathingwater-estonia.geojson https://raw.githubusercontent.com/geopython/diving-into-pygeoapi/tiles-update/workshop/exercises/data/tartu/bathingwater-estonia.geojson
    python3 /load_es_data.py /tmp/bathingwater-estonia.geojson id

```

Above we are downloading the `bathingwater-estonia.geojson` inside the container, and ingesting it into an Elasticsearch index. After this we need to build the docker image:

<div class="termy">
```bash
docker compose build
```
</div>

Edit the `docker.config.yml` configuration on the `pygeoapi` folder, adding this code block before the end:

``` {.yaml linenums="1"}
    bathingwater-estonia:
        type: collection
        title: Bathing water sources
        description: Data of bathing water sources used by water supply systems under the supervision of the Health Board from the Water Health Information System.
        keywords:
          - Water
          - Water bodies
          - Drilled wells
          - Surface water
          - Groundwater
          - Environmental health
          - Health
          - Bathing water
        links:
            - type: text/html
              rel: canonical
              title: information
              href: https://avaandmed.eesti.ee/api/datasets/slug/supluskohad
              hreflang: en-US
        extents:
            spatial:
                bbox: [22.2290936066586440,57.6912449743385451,28.2024877654160555,59.6097269178904412]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
            temporal:
                begin: null
                end: null  # or empty
        providers:
            - type: feature
              name: Elasticsearch
              #Note elastic_search is the docker container of ES the name is defined in the docker-compose.yml
              data: http://elastic_search:9200/bathingwater-estonia
              id_field: id
            - type: tile
              name: MVT-elastic
              data: http://elastic_search:9200/bathingwater-estonia/_mvt/geometry/{z}/{x}/{y}?grid_precision=0
              # index must have a geo_point
              options:
                zoom:
                    min: 0
                    max: 16
              format:
                    name: pbf
                    mimetype: application/vnd.mapbox-vector-tile
```

This configuration enables publishing `bathingwater-estonia.geojson` as both, OGC API - Features and OGC API - Tiles.

Finally start the docker composition, which will download and ingest the dataset and publish it in pygeoapi:

<div class="termy">
```bash
docker compose up
```
</div>

!!! note

    You can check your elastic index at:
    [http://localhost:9200/_cat/indices](http://localhost:9200/_cat/indices)

    If you are in production, you may want to close the elastic ports on docker-compose.

## Client access

### QGIS

QGIS supports OGC API Vector Tiles via the [Vector Tiles Layer](https://docs.qgis.org/3.34/en/docs/user_manual/working_with_vector_tiles/vector_tiles_properties.html). Although OGC API - Tiles are not natively supported, you can customize the `generic connection` in order to access them in QGIS.

!!! question "Access OGC API Vector Tiles from QGIS"

    Before entering QGIS, access your pygeoapi installation page on the browser and follow these steps.

    - access the collection page of the tiles dataset: <http://localhost:5000/collections/bathingwater-estonia>
    - navigate to the tiles page by clicking on `tiles`: <http://localhost:5000/collections/bathingwater-estonia/tiles>
    - click in `Tiles metadata`: <http://localhost:5000/collections/bathingwater-estonia/tiles/WebMercatorQuad/metadata>
    - note the URL template: `http://localhost:5000/collections/bathingwater-estonia/tiles/WebMercatorQuad/{tileMatrix}/{tileRow}/{tileCol}?f=mvt` and of the values of minZoom and maxZoom

    Follow these steps to connect to a service and access vector tiles:

    - locate the vector tiles service on the left hand side browser panel. Note that you can also use the top menu and navigate to `Layer > Add Layer > Vector Tile Layer`

    ![](../assets/images/qgis-vtiles1.png){ width=100% }

    - right-click to bring up the context menu and choose `New Generic connection`
    - fill the required values. For URL, use the URL you noted from the previous step, replacing `{tileMatrix}/{tileRow}/{tileCol}` with `{z}/{x}/{y}`.
    - press `OK` to add the service. At this point, if you are using the browser you should see the collection appearing in the menu, below "Vector Tiles"
    - double-click in the collection to add it to the map
    <!-- - remember to set the CRS of the map to `EPSG:4326` by clicking in the button on the lower right corner -->
    - zoom in to Estonia to visualize your dataset

    ![](../assets/images/qgis-vtiles2-estonia.png){ width=100% }

### LeafletJS

[LeafletJS](https://leafletjs.com) is a popular JavaScript library to add interactive maps to websites. LeafletJS does not support OGC API's explicitely, however can interact with OGC API by using the results of the API directly.

!!! question "Add OGC API - Tiles to a website with LeafletJS"

    * copy the HTML below to a file called `vector-tiles.html`, or locate this file in `workshop/exercises/html`
    * open the file in a web browser

    The code uses the LeafletJS library with the [leaflet.vectorgrid](https://github.com/Leaflet/Leaflet.VectorGrid) plugin to display the lakes OGC API - Tiles service on top of a base layer.

``` {.html linenums="1"}
    <html>
    <head><title>OGC API - Tiles exercise</title></head>
    <body>
    <div id="map" style="width:100vw;height:100vh;"></div>

    <!-- load leaflet -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css"
        integrity="sha512-xodZBNTC5n17Xt2atTPuE1HxjVMSvLVW9ocqUKLsCC5CXdbqCmblAshOMAS6/keqq/sMZMZ19scR4PsZChSR7A=="
        crossorigin="" />
    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"
        integrity="sha512-XQoYMqMTK8LvdxXYG3nZ448hOEQiglfqkJs1NOQV44cWnUrBc8PkAOcXy20w0vlaXaVUearIOBhiXZ5V3ynxwA=="
        crossorigin=""></script>

    <!-- load VectorGrid extension -->
    <script src="https://unpkg.com/leaflet.vectorgrid@1.3.0/dist/Leaflet.VectorGrid.bundled.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/github-fork-ribbon-css/0.2.3/gh-fork-ribbon.min.css" />

    <script>    
    map = L.map('map').setView({ lat: 58.37, lng: 26.72 }, 7);
    map.addLayer(
        new L.tileLayer('https://server.arcgisonline.com/ArcGIS/rest/services/NatGeo_World_Map/MapServer/tile/{z}/{y}/{x}', {
        attribution: 'Tiles &copy; Esri &mdash; National Geographic, Esri, DeLorme, NAVTEQ, UNEP-WCMC, USGS, NASA, ESA, METI, NRCAN, GEBCO, NOAA, iPC',
        minZoom: 1,
        maxZoom: 16,
        }));
    function getColor(val){
        if (val < 0) {return "#ffffff"}
        else if (val < 400) {return "#ffbfbf"}
        else if (val < 1500) {return "#ff8080"}
        else if (val < 3000) {return  "#ff4040"}
        else return "#ff0000";
    }
    var vectorTileStyling = {
        bathingwaterestonia: function(properties) {
            console.log(properties)
            return ({
                fill: true,
                fillColor: getColor(properties.visitors),
                color: "#ffffff",
                fillOpacity: 1.0,
                weight: 1,
                opacity: 1.0,
            });
        }
    } 
        var mapVectorTileOptions = {
            rendererFactory: L.canvas.tile,
            interactive: true,
            vectorTileLayerStyles: vectorTileStyling,
            };
        var pbfURL='http://localhost:5000/collections/bathingwater-estonia/tiles/WebMercatorQuad/{z}/{x}/{y}?f=mvt';
        var pbfLayer=L.vectorGrid.protobuf(pbfURL,mapVectorTileOptions).addTo(map); 
    </script>
    </body>
    </html>
```

In this example, the colors of the symbols reflect the value of the `visitors` attribute.

   ![](../assets/images/leaflet-estonia.png){ width=100% }

!!! note 

    You can check the layer attributes, by opening the console in the developer tools.
    ![](../assets/images/vtiles-attributes.png){ width=100% }


!!! tip 
    Try adding a [different pygeoapi vector tiles layer](https://demo.pygeoapi.io/master/collections/lakes/tiles/WorldCRS84Quad/metadata) by updating the code in `workshop/exercises/html/vector-tiles.html`.

    If you want to render the tiles from the [Elasticsearch example](#publish-vector-tiles-from-elasticsearch), you can check out the code from [this](https://github.com/doublebyte1/vtiles-example/blob/ogcapi-ws/demo-oat.htm) repository:
    <div class="termy">
    ```bash
    git clone -b ogcapi-ws https://github.com/emotional-cities/vtiles-example.git
    ```
    </div>

    ![](../assets/images/leaflet-estonia2.png){ width=100% }

!!! tip 

    See the [official LeafletJS documentation](https://leafletjs.com/reference.html)


### OpenLayers

[OpenLayers](https://openlayers.org) is a popular JavaScript library to add interactive maps to websites. OpenLayers natively supports OGC API - Tiles.

!!! tip 

    See the [official OpenLayers documentation](https://openlayers.org/en/latest/examples/ogc-vector-tiles.html)


# Summary

Congratulations! You are now able to publish tiles to pygeoapi. You can learn more about this standard on: <https://tiles.developer.ogc.org/>
