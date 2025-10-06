s---
title: Exercício 4 - Mapas de dados geoespaciais via OGC API - Maps
---

# Exercício 4 - Mapas de dados geoespaciais via OGC API - Maps

A [OGC API - Maps](https://ogcapi.ogc.org/maps) fornece uma API Web para aceder a quaisquer dados geoespaciais como uma imagem de mapa georreferenciada.

*   [OGC API - Maps](https://docs.ogc.org/DRAFTS/20-058.html)

## Suporte da pygeoapi

A pygeoapi suporta a especificação da OGC API - Maps, utilizando o [MapServer MapScript](https://www.mapserver.org/mapscript) e uma fachada WMS como backends principais.

!!! note

    Consulte [a documentação oficial](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-maps.html) para mais informações sobre os backends de mapa suportados.

## Publicar um conjunto de dados raster

Nesta secção, iremos expor um ficheiro Geopackage disponível na localização `workshop/exercises/data/airport.gpkg` utilizando o [MapServer MapScript](https://www.mapserver.org/mapscript). Estes dados podem ser consumidos por vários clientes que são compatíveis com a OGC API - Maps. Uma lista de alguns desses clientes pode ser encontrada [aqui](https://github.com/opengeospatial/ogcapi-maps/blob/master/implementations.adoc#clients). Aqui também podemos passar o estilo no formato *.sld*, que pode ser gerado no [Geoserver](https://docs.geoserver.org/stable/en/user/styling/index.html), [QGIS](https://www.qgistutorials.com/en/docs/3/basic_vector_styling.html), etc.

!!! question "Interagir com a OGC API - Maps via MapScript"

    Abra o ficheiro de configuração da pygeoapi num editor de texto. Encontre a linha `# START - EXERCISE 4 - Maps`.

    Descomente a secção relacionada com #airports.

    ```{.yaml linenums="1"}
    airports:
        type: collection
        title: airports of the world
        description: Point data representing airports around the world with various metadata such as name, Code, etc.
        keywords:
            - airports
            - natural earth
        links:
            - type: text/html
              rel: canonical
              title: information
              href: https://www.naturalearthdata.com/downloads/10m-cultural-vectors/airports/
              hreflang: en-US
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
            temporal:
                begin:
                end: null  # or empty
        providers:
            - type: map
              name: MapScript
              data: /data/airport.gpkg
              options:
                  type: MS_LAYER_POINT
                  layer: airport
                  style: /data/airport.sld
              format:
                  name: png
                  mimetype: image/png
    ```

!!! note

    Consulte [a documentação oficial](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-maps.html) para mais informações sobre os backends de mapa suportados.

## A pygeoapi como um proxy WMS

Pode consultar a secção "A pygeoapi como uma Ponte para Outros Serviços" para aprender a [publicar WMS como OGC API - Maps](../advanced/bridges.md#publishing-wms-as-ogc-api-maps).

## Acesso do lado do cliente

### QGIS

O QGIS adicionou suporte para APIs que fornecem layers de imagem renderizadas através do seu suporte raster.

!!! question "Adicionar camada da OGC API - Maps ao QGIS"

    - Instale uma versão recente do QGIS (>3.28).
    - Abra o painel `Adicionar camada raster`.
    - Selecione `OGCAPI` para o tipo de Fonte.
    - Adicione o ponto de extremidade local como fonte `http://localhost:5000/collections/airports`.
    - Selecione `PNG` como formato de imagem.
    - Finalmente, adicione a camada ao mapa.

### OWSLib

A [OWSLib](https://owslib.readthedocs.io) é uma biblioteca Python para interagir com os Serviços Web da OGC e suporta várias OGC APIs, incluindo a OGC API - Maps.

!!! question "Interagir com a OGC API - Maps via OWSLib"

    Se não tiver Python instalado, considere executar este exercício num contentor Docker. Consulte o [Capítulo de Configuração](../setup.md#using-docker-for-python-clients).

    === "Linux/Mac"

        <div class="termy">
        ```bash
        pip3 install owslib
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        pip3 install owslib
        ```
        </div>

    Para executar em Python:

    === "Linux/Mac"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.maps import Maps
        >>> m = Maps('http://localhost:5000')
        >>> data = m.map('airports', width=1200, height=800, transparent=False)
        >>> with open("output.png", "wb") as fh:
        ...     fh.write(data.getbuffer())
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.maps import Maps
        >>> m = Maps('http://localhost:5000')
        >>> data = m.map('airports', width=1200, height=800, transparent=False)
        >>> with open("output.png", "wb") as fh:
        ...     fh.write(data.getbuffer())
        ```
        </div>

!!! note

    Consulte a [documentação oficial da OWSLib](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) para mais exemplos.

# Resumo

Parabéns! Agora é capaz de servir um OGC WMS através da pygeoapi e da OGC API - Maps.