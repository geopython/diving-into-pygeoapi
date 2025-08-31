---
Title: Web Semântica e Linked Data
---

# Web Semântica e Linked Data

Esta secção aborda 3 aspetos da Web Semântica:

- [Motores de busca](#motores-de-busca)
- [Publicar dados espaciais na web semântica](#publicar-dados-espaciais-na-web-semântica)
- [Proxy para a web semântica](#proxy-para-a-web-semântica)

## Motores de busca

Os motores de busca usam tecnologia semelhante à Web Semântica para facilitar a captura de dados estruturados (também conhecidos como rich snippets) de páginas web.
A pygeoapi suporta este caso de uso através da incorporação de um snippet JSON-LD `schema.org` na codificação HTML,

!!! tip

    A ontologia `schema.org` não é uma ontologia formal da Web Semântica, estando portanto um pouco desconectada do resto da Web Semântica

!!! tip

    Consulte mais informações em [Otimização para Motores de Busca](./seo.md)

## Publicar dados espaciais na Web Semântica

A OGC API - Common adotou várias convenções W3C, que aproximam as OGC APIs das normas da Web Semântica,
comparado com as normas de primeira geração dos Serviços Web OGC (OWS).

Atualmente, a pygeoapi não pretende ser uma implementação completa da Web Semântica, no entanto é possível anunciar
alguns aspetos da Web Semântica para que os dados possam ser percorridos por clientes conscientes da Web Semântica.

!!! question "Usar um cliente SPARQL para consultar a pygeoapi"

    [SPARQL](https://en.wikipedia.org/wiki/SPARQL) é comummente conhecido como a linguagem de consulta para consultar triple stores. 
    No entanto, também pode usar SPARQL para consultar grafos de recursos web ligados. O cliente SPARQL percorre ligações entre 
    os recursos para localizar os triples solicitados. [Jena ARQ](https://jena.apache.org/documentation/query/) é um cliente 
    SPARQL de linha de comandos que consegue executar tais consultas. O Jena é bastante difícil de configurar, embora haja uma 
    [imagem Docker](https://hub.docker.com/r/stain/jena) disponível. Como alternativa, usaremos uma implementação web 
    do motor ARQ. Navegue para [https://demos.isl.ics.forth.gr/sparql-ld-endpoint](https://demos.isl.ics.forth.gr/sparql-ld-endpoint/)
    e substitua a consulta na caixa de texto por:


    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes> { 
        { 
        ?s ?p ?o  
        } 
      } 
    }
    ``` 

    Uma consulta a um item retorna o item com a sua geometria:

    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes/items/1> {
        {{ ?s ?p ?o }}
      }
    }
    ```

    Note que o cliente SPARQL falha se codificar diretamente o formato HTML. 

    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes?f=html> {
        { ?s ?p ?o }
      }
    }
    ```

    O JSON-LD conforme esperado pelos motores de busca tem alguns desafios para as ferramentas da web semântica. Então como funciona se o formato não for codificado diretamente? 
    O motor SPARQL **negocia** com o endpoint para avaliar que codificações (RDF) estão disponíveis, e baseado na negociação de conteúdo 
    solicita a codificação `JSON_LD` via `f=jsonld`.

A pygeoapi adotou convenções da comunidade [JSON-LD](https://json-ld.org) para anotar JSON como RDF. Para funcionalidades, cada propriedade (coluna numa tabela fonte) 
é anotada por um conceito semântico. A configuração relacionada para aplicar as anotações é gerida no elemento de contexto de uma definição de recurso

!!! tip

    Leia mais na [documentação da pygeoapi](https://docs.pygeoapi.io/configuration#Linked_data).

``` {.yaml linenums="1"}
context:
    - schema: https://schema.org/
    stn_id: schema:identifer
    datetime:
        "@id": schema:observationDate
        "@type": schema:DateTime
    value:
        "@id": schema:value
        "@type": schema:Number
```

## Proxy para a Web Semântica

Os engenheiros de dados espaciais são geralmente desafiados quando importam e visualizam fragmentos da web semântica. O número de 
[clientes espaciais que atualmente suportam interação SPARQL](https://plugins.qgis.org/plugins/sparqlunicorn) é limitado e requer conhecimento especializado para usar. 
Um grupo dentro da comunidade da pygeoapi pretende facilitar o acesso à web semântica para engenheiros de dados espaciais introduzindo a pygeoapi como uma proxy 
entre os clientes GIS típicos e a web semântica.

Uma [nova funcionalidade](https://github.com/geopython/pygeoapi/pull/615) está a ser preparada que introduz um fornecedor SPARQL na pygeoapi. 
O fornecedor permite navegar pelos resultados de uma consulta SPARQL como uma coleção OGC API - Features.

# Resumo

Parabéns! Agora pode configurar configurações da pygeoapi com conceitos de linked data.