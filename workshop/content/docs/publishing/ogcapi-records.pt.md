---
title: Exercício 6 - Metadados via OGC API - Records
---

# Exercício 6 - Metadados via OGC API - Records

[OGC API - Records](https://ogcapi.ogc.org/records) fornece uma Web API com a capacidade de criar, modificar
e consultar metadados na Web:

* Leia a especificação [OGC API - Records: Part 1: Core](https://docs.ogc.org/is/20-004r1/20-004r1.html) no website da OGC.

A OGC API - Records utiliza a OGC API - Features como bloco de construção, permitindo assim implementação
e integração simplificadas para clientes e utilizadores.

## Suporte na pygeoapi

A pygeoapi suporta a especificação OGC API - Records, usando Elasticsearch e TinyDB [rasterio](https://rasterio.readthedocs.io) como backends principais.

!!! note

    Consulte [a documentação oficial](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-records.html) para mais informações sobre backends de catálogo/metadados suportados


## Publicar registos de metadados na pygeoapi

Com a pygeoapi vamos configurar OGC API - Records usando qualquer fornecedor de dados suportado. Neste exercício vamos usar o backend
de catálogo [TinyDB](https://tinydb.readthedocs.io/en/latest/index.html). Vamos utilizar o catálogo de exemplo em `workshop/exercises/data/tartu/metadata/catalogue.tinydb`.

!!! question "Atualizar a configuração da pygeoapi"

    Abra o ficheiro de configuração da pygeoapi num editor de texto. Adicione uma nova secção de conjunto de dados da seguinte forma:

``` {.yaml linenums="1"}
    example_catalogue:
        type: collection
        title: Catálogo nacional da Estónia FOSS4G Europe
        description: Catálogo nacional da Estónia FOSS4G Europe
        keywords:
            - estonia
            - catalogue
            - FOSS4G Europe
        links:
            - type: text/html
              rel: canonical
              title: informação
              href: https://metadata.geoportaal.ee
              hreflang: en-US
        extents:
            spatial:
                bbox: [23.3397953631, 57.4745283067, 28.1316992531, 59.6110903998]
                crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
        providers:
            - type: record
              name: TinyDBCatalogue
              data: /data/tartu/metadata/catalogue.tinydb
              id_field: externalId
              time_field: recordCreated
              title_field: title
```

Guarde a configuração e reinicie o Docker Compose. Navegue para <http://localhost:5000/collections> para avaliar se o novo conjunto de dados foi publicado.

## Formatos de metadados

Por defeito, a pygeoapi suporta e espera o modelo de registo núcleo OGC API - Records e consultáveis. Para formatos de metadados adicionais, pode
desenvolver o seu próprio plugin personalizado da pygeoapi, ou converter os seus metadados para o modelo de registo núcleo OGC API - Records antes de adicionar à pygeoapi.

!!! question "Instalar OWSLib"

    Se não tem Python instalado, considere executar este exercício num contentor Docker. Consulte o [Capítulo de Configuração](../setup.md#using-docker-for-python-clients).

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

### Exemplo de carregador ISO 19139 para TinyDBCatalogue

É possível carregar mais exemplos de metadados ISO19139 numa base de dados TinyDB com [o seguinte script](https://github.com/geopython/pygeoapi/blob/master/tests/load_tinydb_records.py) ([raw](https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py)):

=== "Linux/Mac"

    <div class="termy">
    ```bash
    cd workshop/exercises/data/tartu/metadata
    curl -O https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py
    python3 load_tinydb_records.py xml catalogue.tinydb
    ```
    </div>

=== "Windows (PowerShell)"

    <div class="termy">
    ```bash
    cd workshop/exercises/data/tartu/metadata
    curl https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py
    python3 load_tinydb_records.py xml catalogue.tinydb
    ```
    </div>

Se não tem curl instalado, copie o URL acima para o seu navegador web e guarde localmente.

Se não tem Python instalado, pode executar o carregador usando o contentor Docker OWSLib. Consulte o [Capítulo de Configuração](../setup.md#using-docker-for-python-clients).

!!! example "Usar o contentor Docker OWSLib para carregar metadados"

    === "Linux/Mac"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker run -it --rm --network=host --name owslib -v $(pwd)/data:/data python:3.10-slim /bin/bash
        pip3 install owslib
        apt-get update -y && apt-get install curl -y
        curl -O https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py
        python3 load_tinydb_records.py /data/tartu/metadata/xml /data/tartu/metadata/catalogue.tinydb
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker run -it --rm --network=host --name owslib -v ${pwd}/data:/data python:3.10-slim /bin/bash
        pip3 install owslib
        apt-get update -y && apt-get install curl -y
        curl -O https://raw.githubusercontent.com/geopython/pygeoapi/master/tests/load_tinydb_records.py
        python3 load_tinydb_records.py /data/tartu/metadata/xml /data/tartu/metadata/catalogue.tinydb
        ```
        </div>

Navegue para <http://localhost:5000/collections/example_catalogue> para avaliar se os novos metadados foram publicados
na coleção.

!!! tip pygeometa

    [pygeometa](https://geopython.github.io/pygeometa) é um pacote Python para gerar metadados para conjuntos de dados geoespaciais.
    A pygeometa permite gerir metadados em simples "ficheiros de controlo de metadados" YAML (MCF), e suporta
    importação, exportação bem como transformações para muitos formatos de metadados geoespaciais. Metadados OGC API - Records
    podem ser produzidos usando pygeometa, seja a partir de ficheiros MCF ou transformando de outros formatos.

    Instale e execute pygeometa conforme abaixo para ter uma ideia dos vários comandos e funcionalidade (bem como,
    consulte o [tutorial](https://geopython.github.io/pygeometa/tutorial)).

    === "Linux/Mac"

        <div class="termy">
        ```bash
        pip3 install pygeometa
        pygeometa --help
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        pip3 install pygeometa
        pygeometa --help
        ```
        </div>

## pygeoapi como proxy CSW

Pode consultar a secção "pygeoapi como Ponte para Outros Serviços" para aprender como [publicar CSW como OGC API - Records](../advanced/bridges.md#publishing-csw-as-ogc-api-records).

## Acesso cliente

### QGIS

O QGIS suporta OGC API - Records através do [plugin MetaSearch](https://docs.qgis.org/latest/en/docs/user_manual/plugins/core_plugins/plugins_metasearch.html). O MetaSearch originalmente focava-se apenas no Catalogue Service for the Web (OGC:CSW), mas foi estendido para OGC API - Records. O MetaSearch é um plugin padrão no QGIS e não requer instalação adicional.

!!! question "Consultar OGC API - Records a partir do QGIS"

    Vamos seguir estes passos para nos conectarmos a um serviço e consultar conjuntos de dados:

    - Localize o plugin MetaSearch no menu Web ou na Barra de Ferramentas ![ícone MetaSearch](https://docs.qgis.org/latest/en/_images/MetaSearch.png "ícone MetaSearch"). O painel de pesquisa principal aparecerá com a lista de catálogos MetaSearch padrão já preenchida.

    ![Catálogos pré-preenchidos](../assets/images/prepopulated-catalogues.png){ width=50% }

    - abra o separador `Services`, para encontrar o botão `New` para criar uma nova ligação
    - adicione uma ligação para `https://demo.pygeoapi.io/master`
    - clique em `Service Info` para obter informações sobre o serviço
    - volte ao separador Search
    - selecione a ligação que acabou de criar
    - digite um termo de pesquisa e clique em `search`
    - note que quando seleciona um resultado de pesquisa, uma pegada vermelha é desenhada no mapa destacando a localização do conjunto de dados

    ![Resultados da pesquisa](../assets/images/search-results.png){ width=50% }

[OWSLib](https://owslib.readthedocs.io) é uma biblioteca Python para interagir com Serviços Web OGC e suporta várias OGC APIs incluindo OGC API - Records.

!!! question "Interagir com OGC API - Records via OWSLib"

    Se não tem Python instalado, considere executar este exercício num contentor Docker. Consulte o [Capítulo de Configuração](../setup.md#using-docker-for-python-clients).

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

    Depois inicie uma sessão de consola Python com `python3` (pare a sessão digitando `exit()`).

    === "Linux/Mac"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.records import Records
        >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
        >>> w = Records(SERVICE_URL)
        >>> w.url
        'https://demo.pygeoapi.io/master'
        >>> dutch_metacat = w.collection('dutch-metadata')
        >>> dutch_metacat['id']
        'dutch-metadata'
        >>> dutch_metacat['title']
        'Sample metadata records from Dutch Nationaal georegister'
        >>> dutch_metacat['description']
        'Sample metadata records from Dutch Nationaal georegister'
        >>> dutch_metacat_query = w.collection_items('dutch-metadata', limit=1)
        >>> dutch_metacat_query['numberMatched']
        198
        >>> dutch_metacat_query['numberReturned']
        1
        >>> dutch_metacat_query = w.collection_items('dutch-metadata', q='Wegpanorama')
        >>> dutch_metacat_query['numberMatched']
        2
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```python
        >>> from owslib.ogcapi.records import Records
        >>> SERVICE_URL = 'https://demo.pygeoapi.io/master/'
        >>> w = Records(SERVICE_URL)
        >>> w.url
        'https://demo.pygeoapi.io/master'
        >>> dutch_metacat = w.collection('dutch-metadata')
        >>> dutch_metacat['id']
        'dutch-metadata'
        >>> dutch_metacat['title']
        'Sample metadata records from Dutch Nationaal georegister'
        >>> dutch_metacat['description']
        'Sample metadata records from Dutch Nationaal georegister'
        >>> dutch_metacat_query = w.collection_items('dutch-metadata', limit=1)
        >>> dutch_metacat_query['numberMatched']
        198
        >>> dutch_metacat_query['numberReturned']
        1
        >>> dutch_metacat_query = w.collection_items('dutch-metadata', q='Wegpanorama')
        >>> dutch_metacat_query['numberMatched']
        2
        ```
        </div>

!!! note

    Consulte a [documentação oficial da OWSLib](https://owslib.readthedocs.io/en/latest/usage.html#ogc-api) para mais exemplos.


# Resumo

Parabéns! Agora pode publicar metadados na pygeoapi.