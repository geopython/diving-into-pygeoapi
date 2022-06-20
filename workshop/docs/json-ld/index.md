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

    ARC is a commandline SPARQL client included in jena. Download Jena and locate the ARC client. On console run the following query:

    ```
    
    ``` 

pygeoapi adopted conventions of the [json-ld](https://json-ld.org) community to annotate json as RDF. Each property (column in a source table) is annotated by a semantic concept. The configuration how to apply the annotations is managed in context element in the pygeoapi config file. Read more in the [pygeoapi documentation](https://docs.pygeoapi.io/configuration#Linked_data).

```
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

The typical spatial data engineer is typically challenged to import and visualise fragments of the semantic web. The number of spatial clients supporting SQARQL interaction is limited and requires expert knowledge to use. A group within the pygeoapi community aims to facilitate semantic web access for spatial data engineers by introducing pygeoapi as a proxy between the typical GIS clients and the semantic web.

A new feature is being prepared which introduces a SPARQL provider to pygeoapi. The provider enables to browse the results of a SPARQL query as an OGC API Features collection.
