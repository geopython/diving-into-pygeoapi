---
title: Introduction to pygeoapi
---

# Introduction to pygeoapi

The development team of `pygeoapi` (yes, spelled in lowercase) is excited to welcome you in this workshop! 

In this half day workshop, we will give you an introduction to `pygeoapi`, how to publish data, and provide
resources and tips for future reading and reference (i.e. where to go when you don't know!).

Although `pygeoapi` is written in Python and can be customizable and extensible (plugins) 
for Python developers, Python skills are not required to install, setup and publish your geospatial
data as part of this workshop. All you need for the workshop is your favorite text editor and Docker (we will
more informaiton on Docker in the [setup section](setup.md)). If you need some ideas for a text editor

## Your [FOSS4G 2022](https://2022.foss4g.org) workshop team

<table>
    <tr>
        <td><a href="https://twitter.com/tomkralidis"><img width="150" src="https://avatars.githubusercontent.com/u/910430?v=4"/></a></td>
        <td><a href="https://twitter.com/justb4"><img width="150" src="https://avatars.githubusercontent.com/u/582630?v=4"/></a></td>
        <td><a href="https://twitter.com/pvangenuchten"><img width="150" src="https://avatars.githubusercontent.com/u/299829?v=4"/></a></td>
        <td><a href="https://twitter.com/doublebyte"><img width="150" src="https://avatars.githubusercontent.com/u/1038897?v=4"/></a></td>
        <td><a href="https://twitter.com/francbartoli"><img width="150" src="https://avatars.githubusercontent.com/u/560676?v=4"/></a></td>
        <td><a href="https://twitter.com/tzotsos"><img width="150" src="https://avatars.githubusercontent.com/u/383944?v=4"/></a></td>
        <td><a href="https://twitter.com/PascalLike"><img width="150" src="https://avatars.githubusercontent.com/u/1323093?v=4"/></a></td>
    </tr>
    <tr>
        <td>Tom Kralidis</td>
        <td>Just van den Broecke</td>
        <td>Paul van Genuchten</td>
        <td>Joana Simoes</td>
        <td>Francesco Bartoli</td>
        <td>Angelos Tzotsos</td>
        <td>Antonio Cerciello</td>
    </tr>    
</table>

## Text editor

Your text editor needs to be able to edit configuration files in **plain text**. Examples include, but are
not limited to:

* Notepad or Notepad++ (Windows)
* Sublime Text (Linux/Mac/Windows)
* TextEdit (Mac)
* Visual Studio Code (Linux/Mac/Windows)
* `/usr/bin/vim` (aka [Tom's](https://twitter.com/tomkralidis) favourite :))

## Background reading

The pygeoapi [website](https://pygeoapi.io) is the main entrypoint for both end-users and developers
where you can find:

* all [documentation and presentations](https://pygeoapi.io/documentation)
* our default introductory [default/latest presentation](https://pygeoapi.io/presentations/default)
* our code on [GitHub](https://github.com/geopython/pygeoapi)
* all Docker images [available on Docker Hub](https://hub.docker.com/r/geopython/pygeoapi)

Given `pygeoapi` implements a number of OGC API standards, you may also want to read about these
on [ogcapi.ogc.org](https://ogcapi.ogc.org).

## Existing Deployments

A number of organizations have deployed pygeoapi to their operatios. To get a feel of how `pygeoapi`
is used in practice, check our up to date [live deployments page](https://github.com/geopython/pygeoapi/wiki/LiveDeployments). By
default, the pygeoapi public demo at [demo.pygeoapi.io](https://demo.pygeoapi.io) is always maintained
and made available by the development team. Check out the [main instance](https://demo.pygeoapi.io/master) which
always runs the latest GitHub version.

Interested in the demo site setup itself? demo.pygeoapi.io is developed in a [GitHub repository](https://github.com/geopython/demo.pygeoapi.io) using a
DevOps continuous deployment (CD) workflow.
Even more recent GitOps deployments were developed for [Geonovum](https://github.com/Geonovum/ogc-api-testbed) and the [European Commission Joint Research Center](https://github.com/justb4/ogc-api-jrc).

The above examples may help as starting points for your own `pygeoapi` setup and deployment, so feel free to study and use them!

## History

Starting in 2018, `pygeoapi` emerged as part of the initial efforts for the development of OGC API standards. OGC API
code sprints were instrumental for agile development and pouring the foundation of the project. The core design principles
of the project were and are modularity, extensibility, building by exception, building on a large ecosystem of Free Open Source and OSGeo
components such as GDAL, rasterio, Shapely, Pandas, Elasticsearch, PostGIS and many others.

The project was initiated by [Tom Kralidis](https://github.com/tomkralidis). Within weeks, several talented
developers joined the project, which led to the formation of a core team and [Project Steering Committee (PSC)](https://pygeoapi.io/community/psc). Contributions continued
as well from additional developers and users who happily provided new functionality, bug fixes, and documentation
updates. As a result, a healthy community quickly emerged with a common interest in open source, OGC API standards, low barrier, modular and extensible. The
rest, as they say, is history.

 `pygeoapi` is an [OSGeo Project in incubation](https://www.osgeo.org/projects/pygeoapi) and an
OGC Reference Implementation.
