---
title: Exercício 9 - pygeoapi como ponte para outros serviços
---

# Exercício 9 - pygeoapi como ponte para outros serviços

Nesta secção vamos explorar como a pygeoapi pode ser usada como um interface para re-publicar serviços web com distintas interfaces. Estas pontes podem ajudar [organizações a migrar de OWS para OGC API](https://ogcapi-workshop.ogc.org/transition-and-migration).

## Publicar WFS como OGC API - Features

Um caso de uso importante para a pygeoapi é fornecer uma interface OGC API - Features sobre endpoints Web Feature Service (WFS) 
ou ESRI FeatureServer existentes. Neste cenário, diminui a barreira de acesso e aumenta a usabilidade de serviços existentes para 
uma audiência mais ampla. Vamos configurar uma API sobre um WFS existente (correndo nos servidores da cidade de Florença).

!!! question "Atualizar a configuração da pygeoapi"

    Abra a configuração da pygeoapi num editor de texto. 
    Encontre a linha `# START - EXERCISE 8 - WFS Proxy`.

    Adicione uma nova secção de conjunto de dados descomentando as linhas até `# END - EXERCISE 8 - WFS Proxy`:


    ``` {.yaml linenums="1"}
    suol_epicentri_storici:
        type: collection
        title: Epicentros dos principais sismos históricos
        description: Localização dos epicentros dos principais sismos históricos no território da Cidade Metropolitana de Florença classificados por ano e intensidade
        keywords:
            - sismos
        links:
            - type: text/xml
              rel: canonical
              title: Epicentros dos principais sismos históricos
              href: http://pubblicazioni.cittametropolitana.fi.it/geoserver/territorio/wfs?request=getCapabilities&service=WFS&version=2.0.0
              hreflang: it
        extents:
            spatial:
                bbox: [10.94, 43.52, 11.65, 44.17]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
              name: OGR
              data:
                  source_type: WFS
                  source: WFS:http://pubblicazioni.cittametropolitana.fi.it/geoserver/territorio/wfs?
                  source_capabilities:
                      paging: True
                  source_options:
                      OGR_WFS_LOAD_MULTIPLE_LAYER_DEFN: NO
                  gdal_ogr_options:
                      EMPTY_AS_NULL: NO
                      GDAL_CACHEMAX: 64
                      CPL_DEBUG: NO
              id_field: cpti_id
              crs:
                - http://www.opengis.net/def/crs/OGC/1.3/CRS84
                - http://www.opengis.net/def/crs/EPSG/0/4258
                - http://www.opengis.net/def/crs/EPSG/0/3857
                - http://www.opengis.net/def/crs/EPSG/0/3003
              storage_crs: http://www.opengis.net/def/crs/EPSG/0/3003
              title_field: d
              layer: territorio:suol_epicentri_storici
    ```

Guarde o ficheiro e reinicie o Docker Compose. Navegue para <http://localhost:5000/collections>
para avaliar se o novo conjunto de dados foi publicado.
 
Note estes importantes excertos de configuração sob `providers`:

* Usamos o [Fornecedor OGR](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#ogr) da pygeoapi. 
Este é o backend mais versátil da pygeoapi para suportar numerosos formatos. Usar a biblioteca GDAL/OGR (bindings Python) permite à pygeoapi conectar-se a [cerca de 80+ Formatos Vetoriais](https://gdal.org/drivers/vector).
Podíamos ter usado o Fornecedor `OGR` em vez do Fornecedor `SQLiteGPKG` acima no exercício `osm_places-vec` acima.

* `storage_crs` denota o CRS (Sistema de Referência de Coordenadas) no qual o conjunto de dados é armazenado (o padrão é CRS84, ou seja, 'longitude, latitude') 
* `crs` é um array de CRSs que podem ser especificados para as Funcionalidades serem devolvidas (parâmetro `crs=`), ou para a sua bounding box (parâmetro `bbox-crs=`). O padrão também é CRS84.
 
O suporte CRS permite efetivamente à pygeoapi *reprojetar* os dados do seu CRS de armazenamento (aqui EPSG:3003)
de acordo com [OGC API - Features - Part 2: Coordinate Reference Systems by Reference](https://docs.opengeospatial.org/is/18-058r1/18-058r1.html).
A secção Avançada desta workshop [elaborará o suporte CRS da pygeoapi](../advanced/crs.md).


## Publicar WMS como OGC API - Maps

Podemos usar o provider/fornecedor WMSFacade da pygeoapi para publicar interfaces OGC Web Map Service (WMS) como OGC API - Maps.

 Vamos configurar uma API sobre um WMS existente no Servidor de Demonstração MapServer:
 
 <https://demo.mapserver.org/cgi-bin/msautotest>


!!! note

    Sinta-se à vontade para usar um WMS à sua escolha!

!!! question "Atualizar a configuração da pygeoapi"

    Abra a configuração da pygeoapi num editor de texto. 
    Encontre a linha `## START - EXERCISE 8 - WMS Proxy`.

    Adicione uma nova secção de conjunto de dados descomentando as linhas até `## END - EXERCISE 8 - WMS Proxy`:

     Certifique-se de manter a indentação YAML adequada.

    ``` {.yaml linenums="1"}
    wms-facade-demo:
        type: collection
        title: Demonstração WMS Facade
        description: Demonstração WMS Facade
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

Execute os seguintes pedidos no seu navegador web:

- mapa padrão: <http://localhost:5000/collections/wms-facade-demo/map?f=png>
- largura/altura específicas: <http://localhost:5000/collections/wms-facade-demo/map?f=png&width=800&height=600>
- área de interesse específica (bbox do Canadá): <http://localhost:5000/collections/wms-facade-demo/map?f=png&width=800&height=600&bbox=-142,42,-52,84>

![](../assets/images/maps-response.png){ width=80% }

!!! tip

    Experimente com os seus próprios valores de bbox e largura/altura!

## Publicar CSW como OGC API - Records

Nesta secção veremos como publicar Catalogue Service for the Web (CSW) como OGC API - Records. Para isso, usaremos o serviço CSW [pycsw OGC CITE demo](https://demo.pycsw.org/cite/).

!!! question "Atualizar a configuração da pygeoapi"

    Abra a configuração da pygeoapi num editor de texto. 
    Encontre a linha `# START - EXERCISE 8 - CSW Proxy`.

    Adicione uma nova secção de conjunto de dados descomentando as linhas até `# END - EXERCISE 8 - CSW Proxy`:

    ``` {.yaml linenums="1"}
    cite_demo:
        type: collection
        title: pycsw OGC CITE demo e Implementação de Referência
        description: pycsw é uma implementação de servidor OARec e OGC CSW escrita em Python. O pycsw implementa totalmente a OGC API - Records e a OpenGIS Catalogue Service Implementation Specification (Catalogue Service for the Web). O desenvolvimento inicial começou em 2010 (anunciado mais formalmente em 2011). O projeto é certificado OGC Compliant, e é uma Implementação de Referência OGC. Desde 2015, o pycsw é um Projeto OSGeo oficial. O pycsw permite a publicação e descoberta de metadados geoespaciais através de numerosas APIs (CSW 2/CSW 3, OpenSearch, OAI-PMH, SRU). Repositórios existentes de metadados geoespaciais também podem ser expostos, fornecendo um componente de metadados e catálogo baseado em normas de infraestruturas de dados espaciais. O pycsw é Open Source, lançado sob uma licença MIT, e executa em todas as principais plataformas (Windows, Linux, Mac OS X)
        keywords:
            - ogc
            - cite
            - conformidade
            - interoperabilidade
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: record
              name: CSWFacade
              data: https://demo.pycsw.org/cite/csw
              id_field: identifier
              time_field: datetime
              title_field: title
    ```

Pode explorar a coleção de catálogo com proxy usando estes endpoints:

* página de metadados da coleção: <http://localhost:5000/collections/cite_demo>
* lista de registos: <http://localhost:5000/collections/cite_demo/items>
* registo: <http://localhost:5000/collections/cite_demo/items/urn:uuid:19887a8a-f6b0-4a63-ae56-7fba0e17801f>

!!! tip

    Lembre-se de que pode usar o cliente QGIS sugerido [aqui](https://dive.pygeoapi.io/publishing/ogcapi-records/#client-access) para explorar esta API.

## Publicar SensorThings API como OGC API - Features

A [norma OGC SensorThings API](https://ogcapi-workshop.ogc.org/api-deep-dive/sensorthings/) oferece interfaces RESTful para interconectar dispositivos IoT, dados, de forma aberta e unificada. Embora existam alguns clientes que suportam esta norma, há muito mais que suportam OGC API - Features.

A ponte SensorThings da pygeoapi permite fazer proxy das entidades SensorThings (por exemplo: `Thing` , `Sensor`, `DataStream`, `ObservedProperty` ) em coleções de funcionalidades.

Nesta secção veremos como Publicar um `Thing` da SensorThings API como uma coleção OGC API - Features, que pode então ser consumida por vários clientes, como [os listados aqui](../publishing/ogcapi-features.md#client-access)

!!! question "Atualizar a configuração da pygeoapi"

    Abra a configuração da pygeoapi num editor de texto. 
    Encontre a linha `# START - EXERCISE 8 - SensorThings Proxy`.

    Adicione uma nova secção de conjunto de dados descomentando as linhas até `# END - EXERCISE 8 - SensorThings Proxy`:

    ``` {.yaml linenums="1"}
    toronto_bikes:
        type: collection
        title: Toronto Bikes SensorThings
        description: A localização geográfica com coordenadas para a estação de partilha de bicicletas de Toronto
        keywords:
            - sedimentos
        extents:
            spatial:
                bbox: [-180,-90,180,90]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: feature
              name: SensorThings
              data: https://toronto-bike-snapshot.sensorup.com/v1.0/
              entity: Things
    ```