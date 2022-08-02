---
Title: pygeoapi and the semantic web
---

# pygeoapi and the semantic web

The work on pygeoapi touches on 3 aspects of semantic web:

- [Search engines](#Search_engines)
- [Publish spatial data in the semantic web](#Publish_spatial_data_in_the_semantic_web)
- [Proxy to semantic web](#Proxy_to_semantic_web)


## Search engines

Search engines use technology similar to semantic web, to facilitate capturing structured data (aka rich snippets) from web pages. pygeoapi supports this use case via embedding a schema.org oriented json-ld snippet in its html output, read more at [../seo/index](Search Engine Optimisation). The schema.org ontology is not a formal semantic web ontology, it is therefore disconnected from the rest of the semantic web.

## Publish spatial data in the semantic web

OGC API Common adopted a number of W3C conventions, which brings the API's closer to those of semantic web. At this moment pygeaopi does not aim to be a full implementation of semantic web, however it is possible to advertise some aspects of semantic web, so the data can be traversed by a semantic aware client.

!!! question "Use a SPARQL client to query pygeoapi"

    [SPARQL](https://en.wikipedia.org/wiki/SPARQL) is commonly known as the query language to query triple stores. However you can also use SPARQL to query graphs of linked web resources. The SPARQL client traverses links between the resources to locate the requested triples. [Jena ARQ](https://jena.apache.org/documentation/query/) is a command line SPARQL client which is able to run such queries. Jena is quite difficult to set up, although there is a [Docker Image](https://hub.docker.com/r/stain/jena) available. As an alternative we'll use a webbased implementation of the ARQ engine. Navigate to https://demos.isl.ics.forth.gr/sparql-ld-endpoint/ and replace the query in the textbox with:


    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes> { 
        { 
        ?s ?p ?o  
        } 
      } 
    }
    ``` 

    A query to an item returns the item with its geometry:

    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes/items/1> {
        {{ ?s ?p ?o }}
      }
    }
    ```

    Notice that the SPARQL client fails if you hardcode the html format (which has the jsonld snippet embedded). 

    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes?f=html> {
        { ?s ?p ?o }
      }
    }
    ```

    Jsonld as expected by search engines has some challenges for semantic web tools. So why does it work if the format is not hardcoded? The SPARQL engine `negotiates` with the endpoint to evaluate which (rdf) encodings are available, and based on the content negotiation it requests the `jsonld` encoding.

pygeoapi adopted conventions of the [json-ld](https://json-ld.org) community to annotate json as RDF. Each property (column in a source table) is annotated by a semantic concept. The configuration how to apply the annotations is managed in the context element in the pygeoapi config file. Read more in the [pygeoapi documentation](https://docs.pygeoapi.io/configuration#Linked_data).

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

## Proxy to semantic web

Spatial data engineers are generally challenged when importing and visualising fragments of the semantic web. The number of spatial [clients supporting SQARQL](https://plugins.qgis.org/plugins/sparqlunicorn/) interaction is limited and requires expert knowledge to use. A group within the pygeoapi community aims to facilitate semantic web access for spatial data engineers by introducing pygeoapi as a proxy between the typical GIS clients and the semantic web.

A [new feature](https://github.com/geopython/pygeoapi/pull/615) is being prepared which introduces a SPARQL provider to pygeoapi. The provider enables to browse the results of a SPARQL query as an OGC API Features collection.
