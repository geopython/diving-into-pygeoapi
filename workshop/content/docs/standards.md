---
title: Standards
---

# Standards

pygeoapi is build around the recent set of [API standards](https://ogcapi.ogc.org) emerging at the [Open Geospatial Consortium](https://www.ogc.org/) (OGC).
This section introduces you in an interactive way to the core set of standards on which the new API standards are build.

## OGC API Common

[OGC API Common](https://ogcapi.ogc.org/common/) is a common framework used in all OGC API's. 
Common defines aspects such as:

- OGC API's are based on [Open API 3.0](https://spec.openapis.org/oas/latest.html)
- HTML and JSON are dominant encodings, alternative encodings are possible
- OGC API's have common endpoints such as /conformance, /openapi, /collections/xxx/items/yyy 
- Manages aspects such as pagination, links between resources and basic filters.

Typical for all OGC API's is that the core functionality is kept minimal and extra functionalities 
are added using extensions. The `/conformance` endpoint indicates which standards and extensions are 
supported by a deployment of OGC API.

!!! question "Use web browser to access OGC API"

    Use your web browser to navigate to [demo.pygeoapi.io](https://demo.pygeoapi.io/master). A browser by default opens 
    any OGC API in html (as a webpage) due to the [accept header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept) 
    sent by the browser: 'text/html'. On the right top corner you will notice a `json` link. The link 
    adds the parameter to the url: `?f=json`, which is a mechanism of pygeoapi to override the accept 
    header sent by the browser.

!!! note 

    When calling an OGC API from javascript, and the aim is to receive json. You can use the `?f=json` pygeoapi convention, or the content 
    negotiation as provided by the standard; include a header `accept:"application/json"` in your request.
    In jquery for example, this is represented by the dataType property

    ``` {.js linenums="1"}
    $.ajax({
        method: "GET",
        url: "https://demo.pygeoapi.io/master",
        dataType: "json"
    });
    ```


## Open API

OGC API Common adopted the conventions of the [Open API initiative](https://www.openapis.org/about) 
as a starting point. Any Open API defines its structure in an Open Api Specification document. 
OGC API Common suggests this document to be located at `/openapi`. With pygeoapi in a browser 
[this url](https://demo.pygeoapi.io/master/openapi) opens an interactive html page which facilitates 
to query the api. Append ?f=json to view the document in json. The Open API Specification (OAS) 
document indicates which endpoints are available in the service, which parameters it accepts and 
what type of responses can be expected. You can compare it to the GetCapabilities operation in the 
OWS standards. 

!!! question "OpenAPI Specification parsing in a browser" 

    A common approach to interact with Open API's using json is to use a program like 
    [Postman](https://www.postman.com/). Also there are browser plugins which enable to define api 
    requests interactively within a browser. For firefox download the plugin 
    [poster](https://pluginsaddonsextensions.com/mozilla-firefox/poster-mozilla-addon). For Chrome 
    and Edge use [Boomerang](https://microsoftedge.microsoft.com/addons/detail/boomerang-soap-rest-c/bhmdjpobkcdcompmlhiigoidknlgghfo?hl=en-US). 
    In Boomerang you can create individual web requests, but also load the open api specification 
    document and interact with any of the advertised endpoints. 

The OpenAPI community provides various tools, such as a validator for OAS documents or 
[generate code](https://swagger.io/tools/swagger-codegen/) as a starting point for client development.

## GeoJSON

Up till now the [GeoJSON](https://geojson.org/) format is mainly used as output format of OGC API. The GeoJSON format 
has been enriched with some properties to facilitate linkage, pagination and filters. A minimal example of the GeoJSON output of OGC API Features is shown below:

``` {.json  linenums="1"}
{
    "type": "FeatureCollection",
    "features": [
        {
            "type": "Feature",
            "id": "371",
            "geometry": { "type": "Point", "coordinates": [-75.0, 45.0] },
            "properties": { "stn_id": "35", "value": "89.9" }
        }
    ],
    "numberMatched": 1,
    "numberReturned": 1,
    "links": [
        { "type": "application/geo+json", "rel": "self", "title": "This document as GeoJSON", "href": "https://demo.pygeoapi.io/master/collections/obs/items?f=json&limit=1" },
        { "type": "text/html", "rel": "alternate", "title": "This document as HTML", "href": "https://demo.pygeoapi.io/master/collections/obs/items?f=html&limit=1" },
        { "type": "application/geo+json", "rel": "next", "title": "items (next)", "href": "https://demo.pygeoapi.io/master/collections/obs/items?offset=1&limit=1" },
        { "type": "application/json", "title": "Observations", "rel": "collection","href": "https://demo.pygeoapi.io/master/collections/obs/items"}
    ],
    "timeStamp": "2022-08-04T12:11:19.236327Z"
}
```

At OGC a group works on a new json format for [features and geometries](https://www.ogc.org/projects/groups/featgeojsonswg), 
which will improve some of the current limitations of GeoJSON, such as the limitation of using ESSG:4326 only. 

## Dedicated OGC API's

In the next section we will dive into the dedicated API's related to specific types of information. Notice that multiple of these dedicated api's are combined into a single OGC API endpoint.

- [OGC API Features](publish/ogcfeat.md) provides access to vector features.
- [OGC API Tiles](publish/ogctile.md) provides access to sets of tile imagery or vector
- [OGC API Coverages](publish/ogccov.md) provides access to raster data
- [OGC API Records](publish/ogcrec.md) provides access to repositories of metadata records. 
- [OGC API EDR](publish/ogcedr.md) provides access to environmental data

In the OGC community, new extensions to OGC API emerge regularly. Some of those have an initial phase of implementation in pygeoapi, but we'll not introduce them in this training. Have a look at the software manual to follow their progress.

- [Maps](https://ogcapi.ogc.org/maps) can serve spatially referenced and dynamically rendered map imagery.
- [Processes](https://ogcapi.ogc.org/processes) provides a standardised processing interface as part of OGC API.
- [Routes](https://ogcapi.ogc.org/routes) provides acces to routing data.
- [Spatiotemporal Asset Catalog](https://stacspec.org) provides access to metadata and data about Earth Observation data.
- [Styles](https://ogcapi.ogc.org/styles) defines a Web API that enables map servers, clients as well as visual style editors, to manage and fetch styles.
- [3D GeoVolumes](https://ogcapi.ogc.org/geovolumes) facilitates efficient discovery of and access to 3D content in multiple formats based on a space-centric perspective.
- [Moving Features](https://ogcapi.ogc.org/movingfeatures) defines an API that provides access to data representing features that move as rigid bodies.
- [Joins](https://ogcapi.ogc.org/joins)  supports the joining of data, from multiple sources, with feature collections or directly with other input files.
- [Discrete Global Grid System](https://ogcapi.ogc.org/dggs) enables applications to organise and access data arranged according to a Discrete Global Grid System (DGGS).
