---
title: Standards
---

# Standards

This topic highlights some of the supported standards in pygeoapi.

## OGC API Common

[OGC API Common](https://ogcapi.ogc.org/common/) is a common framework used in all OGC API's. 
Common defines aspects such as:

- OGC API's are based on [Open API 3.0](https://spec.openapis.org/oas/latest.html)
- HTML and JSON are dominant encodings, alternative encodings are possible
- OGC API's have common endpoints such as /conformance, /collections/xxx/items/yyy 
- Manages aspects such as pagination, links between endpoints and basic filters.

Typical for all OGC API's is that the core functionality is kept minimal and extra functionalities 
are added using extensions. The /conformance endpoint indicates which standards and extensions are 
supported by a deployment of OGC API.

!!! question "Use web browser to access OGC API"

    Use your web browser to navigate to http://demo.pygeoapi.org/master. A browser by default opens 
    any OGC API in html (as a webpage) due to the [accept header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept) 
    sent by the browser: 'text/html'. On the right top corner you will notice a `json` link. The link 
    adds the parameter to the url: `?f=json`, which is a mechanism of pygeoapi to override the accept 
    header sent by the browser.

## Open API

OGC API Common adopted the conventions of the [Open API initiative](https://www.openapis.org/about) 
as a starting point. Any Open API defines its structure in an Open Api Specification document. 
OGC API Common suggests this document to be located at `/openapi`. With pygeoapi in a browser 
[this url](http://demo.pygeoapi.org/master/openapi) opens an interactive html page which facilitates 
to query the api. Append ?f=json to view the document in json. The Open API Specification (OAS) 
document indicates which endpoints are available in the service, which parameters it accepts and 
what type of responses can be expected. You can compare it to the GetCapabilities operation in the 
OWS standards. 

!!! question "OpenAPI Specification parsing in a browser" 

    A common approach to interact with Open API's is to use a program like 
    [Postman](https://www.postman.com/). Also there are browser plugins which enable to define api 
    requests interactively within a browser. For firefox download the plugin 
    [poster](https://pluginsaddonsextensions.com/mozilla-firefox/poster-mozilla-addon). For Chrome 
    and Edge use [Boomerang](https://microsoftedge.microsoft.com/addons/detail/boomerang-soap-rest-c/bhmdjpobkcdcompmlhiigoidknlgghfo?hl=en-US). 
    In Boomerang you can create individual web requests, but also load the open api specification 
    document and interact with any of the advertised endpoints. 

The OpenAPI community provides various tools, such as a validator for OAS documents or 
[generate code](https://swagger.io/tools/swagger-codegen/) as a starting point for client development.

## OGC API Features

[OGC API Features'](https://ogcapi.ogc.org/features/) provides a standardised API to exchange vector 
geometries and their attributes. Up till now the GeoJSON format is mainly used. The GeoJSON format 
has been enriched with some properties to facilitate linkage, pagination and filters. AT OGC a group 
works on a new json format for vector geometries, which improves some of the current limitations of 
GeoJSON, such as the limitation of using ESSG:4326 only.

Some plugins are available for OGC API Features:

- The tranformations plugin enables the import and expot of any data according to dedicated projections.
- The CQL extension adds CQL filtering options. Basic filtering options are available in OGC API common. 
CQL adds filters such as AND, OR, smaller/bigger then, within.

Support for CQL is currently under development in pygeoapi. Verify which filter capabilities are
 available for which backends in [the documentation](https://docs.pygeoapi.io/en/latest/cql.html). 

## OGC API Tiles

[OGC API Tiles](https://ogcapi.ogc.org/tiles/) offers a standardised API for accessing repositories 
of tiled imagery. Either in bitmap or vector format.

OGC API Tiles extends the collection/* url structure. In stead of items, the tilesets are listed under 
collection/example/tiles/*, eg.

```
https://demo.pygeoapi.io/collections/lakes/tiles/WorldCRS84Quad/{tileMatrix}/{tileRow}/{tileCol}?f=mvt
```

pygeoapi is able to advertise an existing tileset as OGC API Tiles. pygeoapi itself does not render 
tiles from source data. Use [tilemill](https://tilemill-project.github.io/tilemill/), 
[Mapproxy](https://mapproxy.org/) or 
[QGIS](https://www.qgistutorials.com/en/docs/creating_basemaps_with_qtiles.html) to generate the tileset.

Notice that the OGC API Tiles url structure is compatible with XYZ layers in common libraries, 
such as OpenLayers, Leaflet and MapML.

## OGC API Coverages

Access to coverage datasets (grids) is managed through the 
[OGC Coverage API](https://ogcapi.ogc.org/coverages/). The API is still under development at the OGC.

## OGC API Records

[OGC API Records](https://ogcapi.ogc.org/records/) provides access to repositories of 
metadata records. The API is still under development at the OGC.

## OGC API EDR

[OGC APi EDR](https://ogcapi.ogc.org/edr/)

## OGC API Processes

[OGC API Processes](https://ogcapi.ogc.org/processes/)

## STAC

[Spatiotemporal Asset Catalog](https://stacspec.org/) 