---
title: Client access and development to interact with pygeoapi
---

# Client access and development to interact with pygeoapi

Since pygeoapi provides standardised web services to data, a range of clients and software libraries are available to interact with web services provided by pygeoapi. In this paragraph we highlight some common client (libraries) and provide some exercises to try them out. The topic is split up by each of the supported OGC API standards.

## QGIS

QGIS was one of the first GIS Desktop clients to add support for OGC API Features. The support has been integrated into the WFS connection panel.

- https://docs.qgis.org/3.22/en/docs/server_manual/services/ogcapif.html

!!! note

    An increasing number of GIS Desktop clients add support for OGC API's in subsequent releases. For example ArcGIS Pro [supports OGC API Features](https://pro.arcgis.com/en/pro-app/2.8/help/data/services/use-ogc-api-services.htm) since release 2.8.

Besides OGC API Features, QGIS also supports OGC API Records via the Metasearch plugin.

!!! question "Query OGC API Records"

    query a ogc-api-records catalogue (would be nice to be able to open a map service from a record, status?)
  - add a ogc-api features layer

## GDAL

OGC API Features
- https://gdal.org/drivers/vector/oapif.html
OGC API Coverages
- GDAL - https://gdal.org/drivers/raster/ogcapi.html

!!! question "OGC API in GDAL"

  - connect to a ogc api features service (ogrinfo; ogr2ogr)
  - connect to a ogc api coverages service (gdalinfo)

## OWSLIB

- https://geopython.github.io/OWSLib/usage.html#ogc-api (+jupyter)

## OpenLayers

- https://github.com/openlayers/openlayers/issues/12387

!!! note

    [Leaflet](https://github.com/opengeospatial/ogcapi-features/blob/master/implementations/clients/leaflet.md)
    [ESRI](https://developers.arcgis.com/javascript/latest/api-reference/esri-layers-OGCFeatureLayer.html)


- https://openlayers.org/en/latest/examples/ogc-vector-tiles.html
- https://openlayers.org/en/latest/examples/ogc-map-tiles.html

!!! question "OGC API in Openlayers"

  - Add a ogc-api-tiles layer; 
  - Add a ogc-api-Features layer;



