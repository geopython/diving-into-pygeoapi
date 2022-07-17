---
title: Tiles of geospatial information
---

# Tiles of geospatial information

On  this section, you will learn how to publish vector tiles using the [OGC API - Tiles](https://github.com/opengeospatial/ogcapi-tiles) candidate standard.

You will publish a vector [dataset](.../../../docker/data/cycle-lanes-firenze.geojson) with cycle paths, from the city of Florence. [Here](.../../../docker/data/cycle-lanes-firenze.qmd) you can find more information about the dataset.

Change to docker directory:

```
cd workshop/docker
```

Generate vector tiles on disk, using [tippecanoe](https://github.com/mapbox/tippecanoe):

```
docker run -it --rm \
  -v ${PWD}/data:/data \
  emotionalcities/tippecanoe \
tippecanoe --output-to-directory=/data/tiles/ --force --maximum-zoom=16 --drop-densest-as-needed --extend-zooms-if-still-dropping --no-tile-compression /data/cycle-lanes-firenze.geojson
```

Add the cycle collection to the ```resources``` section of docker.config.yml:

```
    Cycle:
        type: collection
        title: Cycle Circulation Area in Florence 
        description: Cycle lanes and other cycle paths in the city of Florence.
        keywords:
            - cycle
        links:
            - type: text/html
              rel: canonical
              title: information
              href: http://opendata.comune.firenze.it/?q=metarepo/datasetinfo&id=52d8d3ab-eae5-400e-8561-d974f8612de0
              hreflang: en-US
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
            temporal:
                begin: 2011-11-11
                end: null  # or empty
        providers:
            - type: feature
              name: GeoJSON
              data: /data/cycle-lanes-firenze.geojson
              #id_field: field_1
            - type: tile
              name: MVT
              data: /data/tiles
              #data: tests/data/tiles/DATASET
              options:
                metadata_format: tilejson # default | tilejson
                bounds: [[11.1861935050234251,43.7512761718001855],[11.3125196304517655,43.8129406631082645]]
                zoom:
                    min: 0
                    max: 16
                schemes:
                    - WorldCRS84Quad
              format:
                    name: pbf
                    mimetype: application/vnd.mapbox-vector-tile
```

Start pygeoapi with:
```
docker-compose up
```

You can access the ```Cycle``` collection at this endpoint:

[http://localhost:5000/collections/Cycle](http://localhost:5000/collections/Cycle
)

And the tile metadata at this endpoint:

[http://localhost:5000/collections/Cycle/tiles/WorldCRS84Quad/metadata](http://localhost:5000/collections/Cycle/tiles/WorldCRS84Quad/metadata)

![TileSet](img/vtiles.png)