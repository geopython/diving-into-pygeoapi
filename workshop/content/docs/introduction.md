---
title: Introduction to pygeoapi
---

# Introduction to pygeoapi

The development team of pygeoapi (yes, spelled in lowercase) is excited to welcome you in this workshop! 

In this half day workshop, we will give you an introduction to pygeoapi, how to publish data, and provide
resources and tips for future reading and reference (i.e. where to go when you don't know!).

Although pygeoapi is written in Python and can be customizable and extensible (plugins) 
for Python developers, Python skills are not required to install, setup and publish your geospatial
data as part of this workshop. All you need for the workshop is your favorite text editor and Docker (we will
more information in the [setup section](setup.md)).

## Background reading

The pygeoapi [website](https://pygeoapi.io) is the main entrypoint for both end-users and developers
where you can find:

* [official documentation](https://docs.pygeoapi.io)
* the [default/latest presentation](https://pygeoapi.io/presentations/default)
* [documentation and presentations archive](https://pygeoapi.io/documentation)
* code on [GitHub](https://github.com/geopython/pygeoapi)
* Docker images [available on Docker Hub](https://hub.docker.com/r/geopython/pygeoapi)

Given pygeoapi implements a number of OGC API standards, you may also want to read about these
on [ogcapi.ogc.org](https://ogcapi.ogc.org).

## Existing Deployments

A number of organizations have deployed pygeoapi to their operatios. To get a feel of how pygeoapi
is used in practice, check our up to date [live deployments page](https://github.com/geopython/pygeoapi/wiki/LiveDeployments). By
default, the pygeoapi public demo at [demo.pygeoapi.io](https://demo.pygeoapi.io) is always maintained
and made available by the development team. Check out the [main instance](https://demo.pygeoapi.io/master) which
always runs the latest GitHub version.

Interested in the demo site setup itself? [demo.pygeoapi.io](https://demo.pygeoapi.io) is developed in a [GitHub repository](https://github.com/geopython/demo.pygeoapi.io) using a
DevOps continuous deployment (CD) workflow.
Even more recent GitOps deployments were developed for [Geonovum](https://github.com/Geonovum/ogc-api-testbed) and the [European Commission Joint Research Center](https://github.com/justb4/ogc-api-jrc).

The above examples may help as starting points for your own pygeoapi setup and deployment, so feel free to study and use them!

## History

Starting in 2018, pygeoapi emerged as part of the initial efforts for the development of OGC API standards. OGC API
code sprints were instrumental for agile development and pouring the foundation of the project.

The core design principles are as follows:

- simplicity / low barrier to entry
- long term sustainability
- modularity
- extensibility
- building by exception based on a large ecosystem of Free Open Source and OSGeo
  components such as GDAL, rasterio, Shapely, Pandas, Elasticsearch, PostGIS and many others

The project was initiated by [Tom Kralidis](https://github.com/tomkralidis). Within weeks, several talented
developers joined the project, which led to the formation of a core team and [Project Steering Committee (PSC)](https://pygeoapi.io/community/psc). Contributions continued
as well from additional developers and users who happily provided new functionality, bug fixes, and documentation
updates. As a result, a healthy community quickly emerged with a common interest in open source, OGC API standards, low barrier, modular and extensible. The
rest, as they say, is history.

pygeoapi is an [OSGeo Project](https://www.osgeo.org/projects/pygeoapi) and an OGC Reference Implementation.
