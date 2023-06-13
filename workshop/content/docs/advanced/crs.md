---
title: Coordinate Reference Systems (CRS) Support
---

# CRS support

Starting with version 0.15.0 pygeoapi fully supports [OGC API - Features - Part 2: Coordinate Reference Systems by Reference](https://docs.opengeospatial.org/is/18-058r1/18-058r1.html).
This enables the import and export of any data according to dedicated projections.
A "projection" is specified with a Coordinate Reference System (CRS) identifier. These are in URI-formats
like `http://www.opengis.net/def/crs/OGC/1.3/CRS84` (basically WGS84 in lon, lat axis order)
or the "OpenGIS" format like `http://www.opengis.net/def/crs/EPSG/0/4258`. Note that the "EPSG:"-format like `EPSG:4326`
is outside the scope of the OGC standard.

In particular CRS support allows 

- to specify the CRS in which the data is stored, in pygeoapi the `storageCRS:` config option 
- to specify the list of CRSs in which Feature data can be retrieved, in pygeoapi the `crs:` config option
- to publish these CRSs in the collection metadata
- the `crs=` query parameter for a collection or collection item
- the `bbox-crs=` query parameter to indicate that the `bbox=` parameter is encoded in that CRS
- the HTTP response header `Content-Crs` denotes the CRS of the Feature(s) in the data returned

So although GeoJSON mandates WGS84 in lon,lat order, client and server may still agree
on other CRSs.

Under the hood pygeoapi uses the well-known [pyproj](https://pyproj4.github.io/pyproj/stable/) Python wrapper to the [PROJ](https://proj.org/) library.
                                                                                               
Read more in the pygeoapi documentation in the [CRS Chapter](https://docs.pygeoapi.io/en/latest/crs.html).

# Exercise

Adding CRS support to pygeoapi collections for the `provider` type `feature` is as simple as
for example extending the [Exercise 2](../publishing/ogcapi-features.md) config with this snippet:

```
  crs:
      - http://www.opengis.net/def/crs/OGC/1.3/CRS84
      - http://www.opengis.net/def/crs/EPSG/0/4258
      - http://www.opengis.net/def/crs/EPSG/0/3857
      - http://www.opengis.net/def/crs/EPSG/0/4326
  storage_crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84

```


!!! AxisOrder

    Axis order (are coordinates it lon,lat or lat,lon order?) in projections is often a source of confusion. 
    However the URI format is quite clear on this, at least more than the `EPSG:` format.
    So http://www.opengis.net/def/crs/OGC/1.3/CRS84 is lon, lat order, while
    http://www.opengis.net/def/crs/EPSG/0/4326 is lat, lon order.
    
 
In the config below, we basically indicate that the data is stored in WGS84 (lon, lat axis order) and can be retrieved
in CRSs like `http://www.opengis.net/def/crs/EPSG/0/4258` (ETRS89 lat,lon axis order) etc.

!!! question "Add CRS to a pygeoapi configuration"

    Open the pygeoapi configuration file in a text editor.
    Find the line: 
    "# START - EXERCISE 2 - firenze-terrains" 

    Update the dataset section with CRS support by replacing it with the snippet below:

    ``` {.yaml linenums="1"}
    firenze-terrains-vec:
        type: collection
        title: Administrative boundaries before 2014
        description: Cadastral parcels (terrains) from the cadastre. Territory Agency; SIT and Information Networks;
        keywords:
            - Cadastral parcels
        links:
            - type: text/html
              rel: canonical
              title: Administrative boundaries before 2014
              href: http://dati.cittametropolitana.fi.it/geonetwork/srv/metadata/cmfi:c539d359-4387-4f83-a6f4-cd546b3d8443
              hreflang: it
        extents:
            spatial:
                bbox: [11.23,43.75,11.28,43.78]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
              name: SQLiteGPKG
              data: /data/firenze_terrains.gpkg # place correct path here
              id_field: fid
              crs:
                - http://www.opengis.net/def/crs/OGC/1.3/CRS84
                - http://www.opengis.net/def/crs/EPSG/0/4258
                - http://www.opengis.net/def/crs/EPSG/0/3857
                - http://www.opengis.net/def/crs/EPSG/0/4326
              storage_crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
              title_field: codbo
              table: firenze_terrains
    ```
 
Now we can inspect the collection metadata and retrieve Features in various CRSs.
We can even do this in the Swagger UI, but using the browser is quite fast and clear.

## Metadata

!!! question "Collection Metadata"

    Open the URL: 
    [http://localhost:5000/collections/firenze-terrains-vec](http://localhost:5000/collections/firenze-terrains-vec)
    Your configured CRSs are displayed at the bottom of the page: "Reference Systems"
    and "Storage CRS".
    
    See these in JSON format, also at the bottom: 
    http://localhost:5000/collections/firenze-terrains-vec?f=json
    ```    
       .
       .
       "crs":[
        "http://www.opengis.net/def/crs/OGC/1.3/CRS84",
        "http://www.opengis.net/def/crs/EPSG/0/4258",
        "http://www.opengis.net/def/crs/EPSG/0/3857",
        "http://www.opengis.net/def/crs/EPSG/0/4326"
       ],
       "storageCRS":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"
     }
    ```

## Reproject Features

!!! question "Using the CRS query parameter"

    Open the URL: 
    [http://localhost:5000/collections/firenze-terrains-vec/items?f=json&crs=http://www.opengis.net/def/crs/EPSG/0/4258](http://localhost:5000/collections/firenze-terrains-vec/items?f=json&crs=http://www.opengis.net/def/crs/EPSG/0/4258)

    This is ETRS89, similar to WGS84, but for the European Continent (Datum) and in lat,lon order. This is e.g. used in INSPIRE.

    See these in JSON format, also at the bottom:

    ```    
    "type":"FeatureCollection",
      "features":[
          {
              "type":"Feature",
              "geometry":{
                  "type":"MultiPolygon",
                  "coordinates":[
                      [
                          [
                              [
                                  43.77805936835436,
                                  11.23486287997071
                              ],
                              [
                                  43.77809089595012,
                                  11.2348943159564
                              ],
                              [
                                  43.77810038978989,
                                  11.23491359066035
                              ],
                              [
                                  43.77705757917591,
                                  11.2368990806804
       .
       .
       "crs":[
        "http://www.opengis.net/def/crs/OGC/1.3/CRS84",
        "http://www.opengis.net/def/crs/EPSG/0/4258",
        "http://www.opengis.net/def/crs/EPSG/0/3857",
        "http://www.opengis.net/def/crs/EPSG/0/4326"
       ],
       "storageCRS":"http://www.opengis.net/def/crs/OGC/1.3/CRS84"
     }
    ```

    If you open the browser development console you can observe the HTTP response header:

    `Content-Crs: <http://www.opengis.net/def/crs/EPSG/0/4258>`

    (The CRS URI is always enclosed in < >)
