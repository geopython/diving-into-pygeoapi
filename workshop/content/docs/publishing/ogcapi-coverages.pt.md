---
title: Exercício 3 - Dados raster via OGC API - Coverages
---

# Exercício 3 - Dados raster via OGC API - Coverages

A [OGC API - Coverages](https://ogcapi.ogc.org/coverages) fornece uma API Web para aceder a dados raster (grelhas, dados de deteção remota, cubos de dados multidimensionais):

*   [OGC API - Coverages](https://docs.ogc.org/DRAFTS/19-087.html) (**rascunho**)

## Suporte da pygeoapi

A pygeoapi suporta a especificação de rascunho da OGC API - Coverages, com o [rasterio](https://rasterio.readthedocs.io) e o [xarray](https://docs.xarray.dev) como backends principais, bem como [CoverageJSON](https://covjson.org) e output nativo.

!!! note

    Consulte [a documentação oficial](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-coverages.html) para mais informações sobre os backends raster suportados.

## Publicar um conjunto de dados raster

Nos exercícios anteriores, demonstrámos os passos envolvidos na publicação de dados vetoriais e na atualização da configuração da pygeoapi. Nesta secção, vamos publicar um ficheiro raster no formato GeoTIFF, a partir de um fornecedor [rasterio](https://rasterio.readthedocs.io).

!!! question "Atualizar a configuração da pygeoapi"

    Abra o ficheiro de configuração da pygeoapi num editor de texto. Adicione uma nova secção de conjunto de dados da seguinte forma:

    ``` {.yaml linenums="1"}
    tartu-ntl:
        type: collection
        title: Amostra de dados de luzes noturnas da NASA Blue Marble sobre a Estónia
        description: Amostra de dados de luzes noturnas da NASA Blue Marble sobre a Estónia
        keywords:
            - Blue Marble
            - Luzes Noturnas
            - NTL
        links:
            - type: text/html
              rel: about
              title: Dados de Luzes Noturnas da NASA Blue Marble
              href: https://appliedsciences.nasa.gov/get-involved/training/english/arset-introduction-nasas-black-marble-night-lights-data
              hreflang: en
        extents:
            spatial:
              bbox: [26.6264,58.32569,26.82632,58.433989]
              crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: coverage
              name: rasterio
              data: /data/tartu/estonia_light.tif # coloque o caminho correto aqui
              format:
                  name: GTiff
                  mimetype: application/tiff
    ```

!!! tip

    A diretiva `format.name` do fornecedor rasterio **requer** um [nome de driver raster GDAL](https://gdal.org/drivers/raster/index.html) válido.

Guarde a configuração e reinicie o Docker Compose. Navegue para <http://localhost:5000/collections> para avaliar se o novo conjunto de dados foi publicado.

## Acesso do lado do cliente

### GDAL/OGR

O [GDAL/OGR](https://gdal.org) fornece suporte para a [OGC API - Coverages](https://gdal.org/drivers/raster/ogcapi.html). Isto significa que pode usar o `gdalinfo` para consultar e converter dados de endpoints da OGC API - Coverages, tal como qualquer outra fonte de dados raster. Isto também significa que pode estabelecer ligações a endpoints da OGC API - Coverages a partir de qualquer software que tenha uma interface para o GDAL, como MapServer, GeoServer, Manifold, FME, ArcGIS, etc.

!!! question "Usar o GDAL para interagir com a OGC API - Coverages"

    - Verifique se tem uma versão recente do GDAL instalada, caso contrário, use o GDAL do OSGeoLive
    - Execute `gdalinfo` na linha de comandos para verificar uma ligação à OGC API - Coverages:

    === "Linux/Mac"

        <div class="termy">
        ```
        gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```
        gdalinfo OGCAPI:https://maps.ecere.com/ogcapi/collections/SRTM_ViewFinderPanorama
        ```
        </div>

### OWSLib

A [OWSLib](https://owslib.readthedocs.io) é uma biblioteca Python para interagir com os Serviços Web da OGC e suporta várias OGC API, incluindo a OGC API - Coverages.

!!! question "Interagir com a OGC API - Coverages via OWSLib"

    Se não tiver o Python instalado, considere executar este exercício num container de Docker. Consulte o [Capítulo de Configuração](../setup.md#using-docker-for-python-clients).

    === "Linux/Mac"
        <div class="termy">
        ```bash
        pip3 install owslib
        ```        </div>

    === "Windows (PowerShell)"
        <div class="termy">
        ```bash
        pip3 install owslib
        ```
        </div>

    Em seguida, inicie uma sessão de consola Python com: `python3` (pare a sessão escrevendo `exit()`).

    === "Linux/Mac"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.coverages import Coverages
        >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
        >>> w = Coverages(SERVICE_URL)
        >>> w.url
        'https://demo.pygeoapi.io/master/'
        >>> gdps = w.collection('gdps-temperature')
        >>> gdps['id']
        'gdps-temperature'
        >>> gdps['title']
        'Amostra do Sistema de Previsão Determinística Global'
        >>> gdps['description']
        'Amostra do Sistema de Previsão Determinística Global'
        >>> schema = w.collection_schema('gdps-temperature')
        >>> len(schema['field'])
        1
        >>> schema['properties']['1']['title']
        'Temperatura [C]'
        >>> schema['properties']['1']['x-ogc-unit']
        '[C]'
        >>> schema['properties']['1']['type']
        'number'
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.coverages import Coverages
        >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
        >>> w = Coverages(SERVICE_URL)
        >>> w.url
        'https://demo.pygeoapi.io/master/'
        >>> gdps = w.collection('gdps-temperature')
        >>> gdps['id']
        'gdps-temperature'
        >>> gdps['title']
        'Amostra do Sistema de Previsão Determinística Global'
        >>> gdps['description']
        'Amostra do Sistema de Previsão Determinística Global'
        >>> schema = w.collection_schema('gdps-temperature')
        >>> len(schema['field'])
        1
        >>> schema['properties']['1']['title']
        'Temperatura [C]'
        >>> schema['properties']['1']['x-ogc-unit']
        '[C]'
        >>> schema['properties']['1']['type']
        'number'
        ```
        </div>

!!! note

    Consulte a [documentação oficial da OWSLib](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) para mais exemplos.

# Resumo

Parabéns! Agora é capaz de publicar dados raster na pygeoapi.