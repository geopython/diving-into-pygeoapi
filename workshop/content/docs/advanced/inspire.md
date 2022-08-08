---
title: INSPIRE support
---

# INSPIRE support

[INSPIRE](https://inspire.ec.europa.eu/) is a European directive on data sharing in the environmental domain. EU memberstates 
have invested almost 20 years of effort to harmonize data in the environmental domain and publish it using OGC standards. 
The directive is at the end of its lifetime, but the expectation is that conventions from the INSPIRE directive will be adopted 
by upcoming directives, such as green deal and open data directives. 

In the last 20 years the IT environment has changed considerably. INSPIRE has followed these developments by adopting a 
series of [Good Practices](https://inspire.ec.europa.eu/portfolio/good-practice-library) which supersede the original 
[Technical Guidelines](https://inspire.ec.europa.eu/Technical-guidelines3).

Some of the recent and upcoming good practices focus on the developments in the OGC API domain. 
One good practice has already been adopted on providing 
[download services using OGC API Features](https://github.com/INSPIRE-MIF/gp-ogc-api-features) 
and others are in preparation, such as the 
[discovery service using OGC API Records](https://github.com/INSPIRE-MIF/gp-ogc-api-records). 
These developments make pygeoapi an interesting option 
for providing INSPIRE services.


## INSPIRE services and their OGC API alternative

INSPIRE services are typically categorised in view services, download services and discovery services. 
Download services are further devided in Vector sources, 
Coverage sources and Sensor sources. The OGC API initiative has alternative api's for each of these. 
The table below highlights for each service type the TG 
recommendation and the relevant Good Practices. 

| Service type                     | TG     | OGC API                           | Good practice status |
| -------------------------------- | ------ | --------------------------------- | -------------------- | 
| Discovery service                | CSW    | OGC API Records                   | [In preparation](https://github.com/INSPIRE-MIF/gp-ogc-api-records) |
| View service                     | WM(T)S | OGC API Maps<br>OGC API Tiles     | Not scheduled<br> [In preparation](https://wikis.ec.europa.eu/display/InspireMIG/69th+MIG-T+meeting+2022-04-01) |
| Download service - Vector        | WFS    | OGC API Features                  | [Adopted](https://github.com/INSPIRE-MIF/gp-ogc-api-features) |
| Download service - Coverage      | WCS    | OGC API Coverages<br> STAC        | Not scheduled<br> [In preparation](https://github.com/INSPIRE-MIF/gp-stac) | 
| Download service - Sensor        | SOS    | OGC API EDR<br> Sensorthings API [^1]  | Not scheduled<br> [Adopted](https://github.com/INSPIRE-MIF/gp-ogc-sensorthings-api) |

[^1]: Sensorthings API is not an OGC API standard and is currently not supported by pygeoapi. It is listed here for completeness. 

!!! note

    When adopting Good Practices, consider that the documentation and tools for validation are still limited. 
    Also the INSPIRE GeoPortal may not be ready to harvest records from an OGC API Records endpoint yet. 

!!! question "Publish metadata documents as an INSPIRE discovery service"

    In this exercise wel will import a folder of metadata documents into a TinyDB database and configure the database as an OGC API Records endpoint. 
    Download the zipfile 'inspire-records.zip' from the repository. Expand the zipfile. The `/tests` folder contains a script 
    [load_tinydb_records.py](https://github.com/geopython/pygeoapi/blob/master/tests/load_tinydb_records.py). The script has 2 parameters:

    ```
    python3 load_tinydb_records.py <path/to/xml-files> <output.db>
    ```

    Now configure [TinyDB as a provider for OGC API Records](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-records.html#tinydbcatalogue). Restart the service and verify the result. Verify also the xml output of some of the records. 


## OGC API and the INSPIRE data models

Most of the INSPIRE datamodels have a hierarchical structure, which is not common in the GeoJSON oriented OGC API community. 
In theory it is possible to provide hierarchical GML from an OGC API endpoint, but there are not many experiences yet. 
Two initiatives may bring improvement to this aspect:

- pygeoapi facilitates to configure a json-ld encoding using an arbitrary ontology. The 
[good practice on semantic web](https://inspire-eu-rdf.github.io/inspire-rdf-guidelines) provides some of the data models
in an RDF ontology. 
- The [good practice on alternative encodings](https://github.com/INSPIRE-MIF/gp-geopackage-encodings) suggests an 
approach to publish datasets using a relational datamodel (GeoPackage), which fits better with the OGC API community.

## OGC API as a codelist registry

A typical use case in INSPIRE is the option to extend an INSPIRE codelist to match a local situation. For that use case the 
extended codelist has to be published in a registry. OGC API Common provides mechanisms to publish lists of concepts as items 
in collections. pygeoapi provides also a mechanism to advertise the concepts using the SKOS ontology via its json-ld 
encoding. In the coincidence that a concept has a geometry property, the codelist can even be published as OGC API Features 
(on a map).

!!! question "Publish a codelist via OGC API"

    An extended codelist `soil-types.csv` has been prepared in CSV format. Use the [CSV provider](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#csv) to publish this dataset in pygeoapi. Which URL would you use to reference a concept in the published list?
