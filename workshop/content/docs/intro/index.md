# Introduction to pygeoapi

The development team of `pygeoapi` (yes, spelled in lowercase) is very happy to welcome you in this workshop! 
In this 4-hour workshop we will give you an introduction to `pygeoapi`.

Although `pygeoapi` is written in Python (v3) and very customizable and extensible (Plugins) 
for Python developers, Python skills are not required to enjoy the product and this workshop.
Your favorite text-editor (and Docker, more in the [setup section](../setup/index.md)) is all you need!

## Background reading

The website [pygeoapi.io](https://pygeoapi.io/) is the main entrypoint for both end-users and developers.
It provides a wealth of information like:

* all [Documentation and Presentations](https://pygeoapi.io/documentation/)
* for an introduction the [default/latest pygeoapi presentation](https://pygeoapi.io/presentations/default/)
* eager developers may want to check-out the [GitHub repository](https://github.com/geopython/pygeoapi)
* all pygeoapi Docker Images are [available on Docker Hub](https://hub.docker.com/r/geopython/pygeoapi)

`pygeoapi` implements a range of OGC (REST) API standards. You may want to read about these very 
recent developments on [ogcapi.ogc.org](https://ogcapi.ogc.org/).

## Existing Deployments

In order to get a feel of how `pygeoapi` is used in practice, 
we welcome you to click through the following applications.

- [GeoMet-OGC-API](https://api.weather.gc.ca) provides public access to the Meteorological Service of Canada (MSC) and Environment and Climate Change Canada (ECCC) data via interoperable web services and application programming interfaces (APIs).
- British Geological Survey provides an [instance of pygeoapi](https://ogcapi.bgs.ac.uk) with a selection of geological data.
- [GeoE3](https://geoe3.eu) is a project to improve governmental geo date sharing in Europe, focusessed on buildings and infrastructure. [Multiple instances](https://geoe3platform.eu/geoe3/roads/) of pygeoapi are used for various themes.
- [geoplatform.gov](https://sit-geoapi.geoplatform.info), a United States federal geospatial data platform, uses amongst others pygeoapi as a data endpoint.
- [demo.pygeoapi.io](https://demo.pygeoapi.io/), the project demo site by the `pygeoapi` development team. Check the [main instance](https://demo.pygeoapi.io/master) which runs the latest GitHub `pygeoapi` version.

The demo.pygeoapi.io site is developed in [a GitHub repository](https://github.com/geopython/demo.pygeoapi.io) using modern "devops" (GitOps) techniques.
Even more recent GitOps deployments were developed for [Geonovum](https://github.com/Geonovum/ogc-api-testbed), a Template repo, and the [European Commission Joint Research Center](https://github.com/justb4/ogc-api-jrc).
These may be starting points for your own `pygeoapi` deployment!

## History

`pygeoapi` emerged from one of the OGC (Open Geospatial Consortium) 
code sprints around the new generation of OGC API standards 
in 2018, and builds on a large set of Open Source and OSGeo components like 
GDAL, rasterio, Shapely, Pandas, Elastic, PostGIS and many more.

The project was initated by [Tom Kralidis](https://github.com/tomkralidis). 
Over the years several seasoned developers joined the project. Other developers happily provided 
contributions (PRs). `pygeoapi` is now an [OSGeo Project](https://www.osgeo.org/projects/pygeoapi/) and 
OGC Reference Implementation.
