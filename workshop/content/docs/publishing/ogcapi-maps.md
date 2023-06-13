---
title: Exercise 5 - Maps of geospatial data via OGC API - Maps
---

# Exercise 5 - Maps of geospatial data via OGC API - Maps

[OGC API - Maps](https://ogcapi.ogc.org/maps) provides a Web API to access
any geospatial data as a georeferenced map image.

* [OGC API - Maps](https://docs.ogc.org/DRAFTS/20-058.html) (**draft**)

## pygeoapi support

pygeoapi supports the OGC API - Maps draft specification, using [MapServer MapScript](https://www.mapserver.org/mapscript) and a WMS facade as core backends.

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-maps.html) for more information on supported map backends

## Serve a WMS via OGC API - Maps

Let's use pygeoapi's WMSFacade provider as a bridge to serve an OGC WMS via OGC API - Maps.

We can use the MapServer demo server at https://demo.mapserver.org/cgi-bin/msautotest

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

## Client access

### OWSLib

[OWSLib](https://owslib.readthedocs.io) is a Python library to interact with OGC Web Services and supports a number of OGC APIs including OGC API - Maps.

!!! question "Interact with OGC API - Maps via OWSLib"

    If you do not have Python installed, consider running this exercise in a Docker container or in a cloud environment.

    <div class="termy">
    ```bash
    pip3 install owslib
    ```
    </div>

    <div class="termy">
    ```python
    >>> from owslib.ogcapi.maps import Maps
    >>> m = Maps('http://localhost:5000')
    >>> data = m.map('wms-facade-demo', width=1200, height=800, transparent=False)
    >>> with open("output.png", "wb") as fh:
    ...     fh.write(data.getbuffer())
    ```
    </div>

!!! note

    See the official [OWSLib documentation](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) for more examples.

# Summary

Congratulations! You are now able to serve an OGC WMS via pygeoapi and OGC API - Maps.
