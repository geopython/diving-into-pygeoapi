---
Title: Semantic Web and Linked Data
---

# Semantic Web and Linked Data

This section touches on 3 aspects of the Semantic Web:

- [Search engines](#search-engines)
- [Publish spatial data in the semantic web](#publish-spatial-data-in-the-semantic-web)
- [Proxy to semantic web](#proxy-to-semantic-web)

## Search engines

Search engines use technology similar to the Semantic Web to facilitate capturing structured data (aka rich snippets) from web pages.
pygeoapi supports this use case via embedding a `schema.org` JSON-LD snippet in the HTML encoding,

!!! tip

    The `schema.org` ontology is not a formal Semantic Web ontology, it is therefore a bit disconnected from the rest of the Semantic Web

!!! tip

    See more information at [Search Engine Optimization](../seo)

## Publish spatial data in the Semantic Web

OGC API - Common adopted a number of W3C conventions, which bring OGC APIs closer to the standards of Semantic Web,
compared to first generation OGC Web Service (OWS) standards.

Currently, pygeaopi does not aim to be a full implementation of Semantic Web, however it is possible to advertise
some aspects of the Semantic Web so the data can be traversed by Semantic Web aware clients.

!!! question "Use a SPARQL client to query pygeoapi"

    [SPARQL](https://en.wikipedia.org/wiki/SPARQL) is commonly known as the query language to query triple stores. 
    However you can also use SPARQL to query graphs of linked web resources. The SPARQL client traverses links between 
    the resources to locate the requested triples. [Jena ARQ](https://jena.apache.org/documentation/query/) is a command 
    line SPARQL client which is able to run such queries. Jena is quite difficult to set up, although there is a 
    [Docker Image](https://hub.docker.com/r/stain/jena) available. As an alternative we will use a web-based implementation 
    of the ARQ engine. Navigate to [https://demos.isl.ics.forth.gr/sparql-ld-endpoint](https://demos.isl.ics.forth.gr/sparql-ld-endpoint/)
    and replace the query in the textbox with:


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

    Notice that the SPARQL client fails if you hardcode the HTML format. 

    ``` {.sql linenums="1"}
    SELECT * WHERE { 
      SERVICE <https://demo.pygeoapi.io/master/collections/lakes?f=html> {
        { ?s ?p ?o }
      }
    }
    ```

    JSON-LD as expected by search engines has some challenges for semantic web tools. So how does it work if the format is not hardcoded? 
    The SPARQL engine **negotiates** with the endpoint to evaluate which (RDF) encodings are available, and based on the content negotiation 
    it requests the `JSON_LD` encoding via `f=jsonld`.

pygeoapi adopted conventions of the [JSON-LD](https://json-ld.org) community to annotate JSON as RDF. For features, each property (column in a source table) 
is annotated by a semantic concept. The related configuration to apply the annotations is managed in the context element of a resource definition

!!! tip

    Read more in the [pygeoapi documentation](https://docs.pygeoapi.io/configuration#Linked_data).

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

## Proxy to the Semantic Web

Spatial data engineers are generally challenged when importing and visualizing fragments of the semantic web. The number of spatial 
[clients currently supporting SQARQL](https://plugins.qgis.org/plugins/sparqlunicorn) interaction is limited and requires expert knowledge to use. 
A group within the pygeoapi community aims to facilitate semantic web access for spatial data engineers by introducing pygeoapi as a proxy 
between the typical GIS clients and the semantic web.

A [new feature](https://github.com/geopython/pygeoapi/pull/615) is being prepared which introduces a SPARQL provider to pygeoapi. 
The provider enables to browse the results of a SPARQL query as an OGC API - Features collection.

# Summary

Congratulations! You can now configure pygeoapi configurations with linked data concepts.
