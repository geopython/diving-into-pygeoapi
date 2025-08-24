---
title: Exercício 1 - O primeiro conjunto de dados
---

# Exercício 1 - O primeiro conjunto de dados

Nesta secção, irá publicar um conjunto de dados vetoriais.

Para este exercício, iremos usar um conjunto de dados CSV de [águas balneares na Estónia](https://github.com/geopython/diving-into-pygeoapi/tree/main/workshop/exercises/data/tartu/bathingwater-estonia.csv), gentilmente cedido pela [Agência de Saúde da Estónia](https://terviseamet.ee).

Pode encontrar este conjunto de dados em `workshop/exercises/data/tartu/bathingwater-estonia.csv`.

Este exercício consiste em ajustar o `workshop/exercises/pygeoapi.config.yml` para definir este conjunto de dados como uma *coleção* da OGC API - Features.

## Verificar a configuração existente do Docker Compose

Antes de fazer quaisquer alterações, vamos garantir que a configuração inicial do Docker Compose que lhe foi fornecida está a funcionar.

Para testar:

!!! question "Testar a configuração do workshop"

    1. Numa shell de terminal, navegue para a pasta do workshop e digite:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker compose up
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker compose up
        ```
        </div>

    1. Abra <http://localhost:5000> no seu navegador, verifique algumas coleções
    1. Feche digitando `CTRL-C`

!!! note

    Também pode executar o contentor Docker em segundo plano (detached) da seguinte forma:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        docker compose up -d
        docker ps  # verifique que o container pygeoapi está em execução
        # visite http://localhost:5000 no seu navegador, verifique algumas coleções
        docker logs --follow pygeoapi  # ver registos
        docker compose down --remove-orphans
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        docker compose up -d
        docker ps  # verifique que o container pygeoapi está em execução
        # visite http://localhost:5000 no seu navegador, verifique algumas coleções
        docker logs --follow pygeoapi  # ver registos
        docker compose down --remove-orphans
        ```
        </div>

## Publicar o primeiro conjunto de dados

Chegou o momento de publicar o seu primeiro conjunto de dados.

!!! question "Configurar o ficheiro de configuração da pygeoapi"

    1. Abra o ficheiro `workshop/exercises/pygeoapi/pygeoapi.config.yml` no seu editor de texto
    1. Procure a secção de configuração comentada que começa com `# START - EXERCISE 1 - Your First Collection`
    1. Descomente todas as linhas até `# END - EXERCISE 1 - Your First Collection`

Certifique-se de que a indentação está alinhada (dica: diretamente abaixo de `# START ...`)

A secção de configuração é a seguinte:

``` {.yml linenums="185"}
    Bathing_Water_Estonia:
        type: collection
        title: Bathing Water Estonia
        description: Locations where the Estonian Health Board monitors the bathing water quality
        keywords:
            - bathing water
            - estonia
        links:
            - type: text/csv
              rel: canonical
              title: data
              href: https://avaandmed.eesti.ee/datasets/supluskohad
              hreflang: EE
        extents:
            spatial:
                bbox: [20,57,29,60]
                crs: http://www.opengis.net/def/crs/EPSG/0/4326
        providers:
            - type: feature
              name: CSV
              data: /data/tartu/bathingwater-estonia.csv
              id_field: id
              title_field: Name
              geometry:
                x_field: x
                y_field: y
              storage_crs: http://www.opengis.net/def/crs/EPSG/0/3300
```

A parte mais relevante é a secção `providers`. Aqui, definimos um `Provider CSV`, apontando o caminho do ficheiro para o diretório `/data` que iremos montar (ver a seguir) do diretório local para o container de Docker. Como um CSV não é um ficheiro espacial, configuramos explicitamente a pygeoapi para que a longitude e a latitude (x e y) sejam mapeadas a partir das colunas `lon` e `lat` no ficheiro CSV. Note o parâmetro `storage_crs`, que indica o sistema de coordenadas que é usado nos dados de origem.

!!! Tip

    Para saber mais sobre a sintaxe e as convenções de configuração da pygeoapi, consulte o [capítulo relevante na documentação](https://docs.pygeoapi.io/en/latest/configuration.html).

!!! Tip

    A pygeoapi inclui [inúmeros fornecedores de dados](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) que permitem o acesso a uma variedade de formatos de dados. Através do plugin OGR/GDAL, o número de formatos suportados é quase ilimitado. Consulte a [página do fornecedor de dados](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) para saber como pode configurar uma ligação ao conjunto de dados da sua escolha. Pode sempre copiar um exemplo de configuração relevante e colocá-lo na secção de conjuntos de dados do ficheiro de configuração da pygeoapi para o seu projeto futuro.

## Testar

!!! question "Iniciar com a configuração atualizada"

    1. Comece por digitar `docker compose up`
    1. Observe o output do registo (logging)
    1. Se não houver erros: abra <http://localhost:5000>
    1. Procure a coleção "Bathing Water Estonia"
    1. Navegue pelos itens da coleção
    1. Verifique a representação json adicionando `?f=json` ao URL (ou clicando em 'json' no canto superior direito)

## Depuração de erros de configuração

Ocasionalmente, pode encontrar erros, brevemente discutidos aqui:

*   Não é possível encontrar um ficheiro, um erro de digitação na configuração
*   O formato ou a estrutura do ficheiro espacial não é totalmente suportado
*   A porta (5000) já está a ser utilizada. Existe uma pygeoapi anterior ainda em execução? Se alterar a porta, considere que também tem de atualizar o ficheiro de configuração da pygeoapi

Existem dois parâmetros no ficheiro de configuração que ajudam a resolver estes problemas.
Defina o nível de registo (logging) para `DEBUG` e indique um caminho para um ficheiro de registo.

!!! tip

    No Docker, defina o caminho do ficheiro de registo para a pasta montada, para que possa acedê-lo facilmente a partir do seu sistema anfitrião. Também pode ver os registos da consola do seu contentor Docker da seguinte forma:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        docker logs --follow pygeoapi
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        docker logs --follow pygeoapi
        ```
        </div>

!!! tip

    Erros relacionados com caminhos de ficheiros ocorrem normalmente na configuração inicial. No entanto, também podem acontecer em momentos inesperados, resultando num serviço interrompido. Produtos como o [GeoHealthCheck](https://geohealthcheck.org) têm como objetivo monitorizar, detetar e notificar sobre o estado e a disponibilidade do serviço. Os testes da OGC API - Features no GeoHealthCheck verificam a disponibilidade do serviço em intervalos. Consulte a [documentação do GeoHealthCheck](https://docs.geohealthcheck.org) para mais informações.