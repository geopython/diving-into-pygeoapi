---
title: Exercise 5 - Maps of geospatial data via OGC API - Maps
---

# Exercise 5 - Maps of geospatial data via OGC API - Maps

[OGC API - Maps](https://ogcapi.ogc.org/maps) provides a Web API to access
any geospatial data as a georeferenced map image.

* [OGC API - Maps](https://docs.ogc.org/DRAFTS/20-058.html) (**draft**)

## pygeoapi support

pygeoapi supports the OGC API - Maps draft specification, using [MapServer MapScript](https://www.mapserver.org/mapscript) and a WMS facade as core backends.

In this section we'll be exposing a shapefile available at `/data/airport/airport.shp` location. This data can be consumed with various clients which can access OGC APIs - Maps. Here we can also pass style in *.sld* format. Which can be generated on [Geoserver](https://docs.geoserver.org/stable/en/user/styling/index.html), [QGIS](https://www.qgistutorials.com/en/docs/3/basic_vector_styling.html), etc.
 

```
providers:
    - type: map
      name: MapScript
      data: /data/airport/airport.shp
      options:
          type: MS_LAYER_POINT
          layer: airport
          style: /data/airp.sld
      format:
          name: png
          mimetype: image/png
```

!!! note

    See [the official documentation](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-maps.html) for more information on supported map backends

## pygeoapi as a WMS proxy

You can check the "pygeoapi as a Bridge to Other Services" section to learn how to [publish WMS as OGC API - Maps](../../advanced/bridges/#publishing-wms-as-ogc-api-maps).

## Client access

### OWSLib

[OWSLib](https://owslib.readthedocs.io) is a Python library to interact with OGC Web Services and supports a number of OGC APIs including OGC API - Maps.

!!! question "Interact with OGC API - Maps via OWSLib"

    If you do not have Python installed, consider running this exercise in a Docker container. See the [Setup Chapter](../setup.md#using-docker-for-python-clients).

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
