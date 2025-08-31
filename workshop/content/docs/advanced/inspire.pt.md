---
title: Suporte INSPIRE
---

# Suporte INSPIRE

[INSPIRE](https://inspire.ec.europa.eu) é uma diretiva europeia sobre partilha de dados no domínio ambiental. Os estados membros da UE 
investiram quase 20 anos de esforço para harmonizar dados no domínio ambiental e publicá-los usando normas OGC. 
A diretiva está no final do seu tempo de vida, mas a expectativa é que as convenções da diretiva INSPIRE sejam adotadas 
por diretivas futuras, como as diretivas do pacto verde e dados abertos. 

Nos últimos 20 anos, o panorama das TI mudou consideravelmente. O INSPIRE acompanhou estes desenvolvimentos adotando uma 
série de [Boas Práticas](https://inspire.ec.europa.eu/portfolio/good-practice-library) que substituem as 
[Diretrizes Técnicas](https://inspire.ec.europa.eu/Technical-guidelines3) originais.

Algumas das boas práticas recentes e futuras focam-se nos desenvolvimentos no domínio OGC API. 
Uma boa prática já foi adotada sobre fornecer 
[serviços de descarregamento usando OGC API - Features](https://github.com/INSPIRE-MIF/gp-ogc-api-features) 
e outras estão em preparação, como o 
[serviço de descoberta usando OGC API - Records](https://github.com/INSPIRE-MIF/gp-ogc-api-records). 
Estes desenvolvimentos tornam a pygeoapi uma opção interessante 
para fornecer serviços INSPIRE.


## Serviços INSPIRE e a sua alternativa OGC API

Os serviços INSPIRE são tipicamente categorizados em serviços de visualização, serviços de descarregamento e serviços de descoberta. 
Os serviços de descarregamento são ainda divididos em fontes Vetoriais, fontes de Cobertura e fontes de Sensores.
A iniciativa OGC API fornece as APIs relacionadas para cada tipo de serviço.
A tabela abaixo destaca para cada tipo de serviço a recomendação das Diretrizes Técnicas
e as Boas Práticas relevantes. 

| Tipo de serviço                      | TG     | OGC API                             | Estado da boa prática |
| ------------------------------------ | ------ | ----------------------------------- | --------------------- | 
| Serviço de descoberta                | CSW    | OGC API - Records                   | [Em preparação](https://github.com/INSPIRE-MIF/gp-ogc-api-records) |
| Serviço de visualização              | WM(T)S | OGC API - Maps / OGC API - Tiles    | Não agendado<br> [Em preparação](https://wikis.ec.europa.eu/display/InspireMIG/69th+MIG-T+meeting+2022-04-01) |
| Serviço de descarregamento - Vetor   | WFS    | OGC API - Features                  | [Adotada](https://github.com/INSPIRE-MIF/gp-ogc-api-features) |
| Serviço de descarregamento - Cobertura | WCS  | OGC API - Coverages / STAC [^1]     | Não agendado<br> [Em preparação](https://github.com/INSPIRE-MIF/gp-stac) | 
| Serviço de descarregamento - Sensor   | SOS   | OGC API - EDR / Sensorthings API [^2] | Não agendado<br> [Adotada](https://github.com/INSPIRE-MIF/gp-ogc-sensorthings-api) |

[^1]: A Sensorthings API não é uma norma OGC API e atualmente não é suportada pela pygeoapi. É listada aqui para completude
[^2]: STAC não é uma norma OGC API mas é suportada pela pygeoapi

!!! note

    Ao adotar Boas Práticas, considere que a documentação e ferramentas para validação ainda são limitadas. 
    Além disso, o Portal Geo INSPIRE pode ainda não estar pronto para recolher registos de um endpoint OGC API - Records. 

!!! question "Publicar documentos de metadados como um serviço de descoberta INSPIRE"

    Neste exercício vamos importar uma pasta de documentos de metadados para uma base de dados TinyDB e vamos configurar a base de dados como um endpoint OGC API - Records. 
    Descarregue o ficheiro zip 'inspire-records.zip' do repositório. Extraia o ficheiro zip. A pasta `/tests` contém um script 
    [load_tinydb_records.py](https://github.com/geopython/pygeoapi/blob/master/tests/load_tinydb_records.py). O script tem 2 parâmetros:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        python3 load_tinydb_records.py <path/to/xml-files> <output.db>
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        python3 load_tinydb_records.py <path/to/xml-files> <output.db>
        ```
        </div>

    Agora configure [TinyDB como fornecedor para OGC API - Records](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-records.html#tinydbcatalogue). Reinicie o serviço e verifique o resultado. Verifique também a saída XML de alguns dos registos. 


## OGC API e os modelos de dados INSPIRE

A maioria dos modelos de dados INSPIRE tem uma estrutura hierárquica, que não é comum na comunidade OGC API orientada para GeoJSON. 
Em teoria é possível fornecer GML hierárquico a partir de um endpoint OGC API, mas ainda não há muitas experiências atualmente.
Duas iniciativas podem trazer melhorias a este aspeto:

- a pygeoapi facilita a configuração de uma codificação JSON-LD usando uma ontologia arbitrária. A 
[boa prática sobre web semântica](https://inspire-eu-rdf.github.io/inspire-rdf-guidelines) fornece alguns dos modelos de dados
numa ontologia RDF
- A [boa prática sobre codificações alternativas](https://github.com/INSPIRE-MIF/gp-geopackage-encodings) sugere uma 
abordagem para publicar conjuntos de dados usando um modelo de dados relacional como GeoPackage, que se adequa melhor à comunidade OGC API

## OGC API como um registo de listas de códigos

Um caso de uso típico no INSPIRE é a opção de estender uma lista de códigos INSPIRE para corresponder a um requisito local. Para este caso de uso, a 
lista de códigos estendida tem de ser publicada num registo. A OGC API - Common fornece mecanismos para publicar listas de conceitos como itens 
em coleções. A pygeoapi também fornece um mecanismo para anunciar os conceitos usando a ontologia SKOS através da sua codificação JSON-LD 
. Na coincidência de um conceito ter uma propriedade de geometria, a lista de códigos pode até ser publicada como OGC API - Features 
(num mapa).

!!! question "Publicar uma lista de códigos via OGC API"

    Uma lista de códigos de tipos de solo alemã foi disponibilizada em formato CSV em `workshop/exercises/data/bodenart.en.csv`. Use o [fornecedor CSV](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#csv) para publicar este conjunto de dados na pygeoapi. Que URL usaria para referenciar um conceito na lista publicada?

``` {.yaml linenums="1"}
SoilTypes:
    type: collection
    title: Tipos de solo da Alemanha
    description: Bodenarten auf Basis der Bodenkundlichen Kartieranleitung 5. Auflage (KA5)
    keywords:
        - soiltype
    links:
        - type: text/html
          rel: canonical
          title: Tipos de solo da Alemanha
          href: https://registry.gdi-de.org/codelist/de.bund.thuenen/bodenart
          hreflang: de
    extents:
        spatial:
            bbox: [0,0,0,0]
            crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
    providers:
        - type: feature
          name: CSV
          data: /data/bodenart.en.csv
          id_field: Label
          geometry:
              x_field: x
              y_field: y
```

# Resumo

Parabéns! Trabalhou com a pygeoapi para conformidade INSPIRE