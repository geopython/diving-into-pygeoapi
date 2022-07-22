---
title: Setup of the workshop environment
---

# Setup
 
In this workshop we will (only) use Docker and Docker Compose.
So the first requirement is to install these on your system.
We strongly advise to do the installation before the workshop starts.

Although several custom installation-methods for `pygeoapi` are available and well-documented 
at [pygeoapi.io](https://pygeoapi.io), these
will *not* be considered in this workshop. 
Exercises will also be based on Docker, hence a custom installation would at least be 'challenging'. 
The good news that only a single installation (Docker) is needed! The Docker Images
will contain the latest `pygeoapi` and all its dependencies and external services like PostGIS.

## About Docker

Docker has been available for almost 10 years, widely used on OSGeo software, so chances are big that you have
heard of Docker and possibly of 'Containerization' in general. Or, better you are familiar and hopefully using Docker already.
If not, there is a plethora of introductory materials like [here](https://www.ibm.com/in-en/cloud/learn/docker).
[Docker.com](https://www.docker.com/) remains the main entry.

Especially for the deployment and integration of Open Source Geospatial components, Docker
is a blessing as it provides consistent packaging, isolation, integration and upgrade patterns, compared
to custom installations. Though today we mainly use Docker, the bigger picture is the use of *Containers* as a next step
in virtualization, but that would deserve its own workshop...

[Docker Compose](https://docs.docker.com/compose/) is an addition to Docker to facilitate 
the orchestration of one or more Docker 'Containers' (a Container is a running instance of a Docker Image) 
using a configuration convention, "The Docker Compose YAML File", usually named `docker-compose.yml`.

Stepping up further are even more sophisticated Docker Orchestrators like Rancher and Kubernetes, but for
our workshop Docker and Docker Compose is all we need.

## Installation

Docker installation has greatly progressed over the years. This is the only part of the workshop
which is dependent on the system/OS you are running: Windows, Mac or Linux. For each
system the Docker website provides detailed installation instructions. Please follow these consistently.
The product is called Docker Desktop and includes Docker Compose:

* Windows [installation](https://docs.docker.com/desktop/install/windows-install/)
* Mac [installation](https://docs.docker.com/desktop/install/mac-install/)
* Linux [installation](https://docs.docker.com/desktop/install/linux-install/)

Some notes:

* on Windows we recommend using the WSL, Windows Subsystem for Linux as it also provides a powerful (Bash) commandline
* on Mac, if you are already using Homebrew, consider (as the author has) using the [brew Docker formula](https://formulae.brew.sh/formula/docker)
* on Linux you need to choose your platform. You could also use Virtualbox with a Ubuntu Image or have on a cloud VM.

If all goes well, you should be able to run Docker from the commandline like:

* `docker --version`
* `docker-compose --version`

The actual versions are not extremely important.

## Quickstart

Once Docker is available on your system, running `pygeoapi` with its built-in configuration and 
data is a one-liner. Open a terminal session:

<div class="termy">

```console
$ docker run --rm -p 5000:80 geopython/pygeoapi:latest
$ curl http://localhost:5000
# or open http://localhost:5000 with your browser
# stop with control-C
```

</div>

That's all! In the first `docker run` it downloads the Docker Image. 
This may take some time, as the Docker Image of `pygeoapi` includes all 
dependencies like GDAL etc. Be patient! This is only once for the entire workshop, or
you may want to do this beforehand. Some notes:


* Runs `pygeoapi` on your local system on port 5000, which is mapped to port 80 inside the Container. 
* Runs with the [default configuration](https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml) and data from the GitHub repo.
* The `--rm` option removes the Docker Container, not the Image, after execution.

Next, you can override the default configuration and add your own data using *Docker Volume mounts*.

### Custom Configuration

Here we override the default configuration which resides at `/pygeoapi/local.config.yml` within the Container 
with our local file [default.config.yml](https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml) 
by using Docker Volume Mount (`-v` option). A Docker Volume mount attaches, 'mounts', a 
directory or single file from your host/local system into the Docker Container.

<div class="termy">

```console
$ curl -O https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml
$ vi default.config.yml
$ docker run -p 5000:80 \
    -v $(pwd)/default.config.yml:/pygeoapi/local.config.xml \
    geopython/pygeoapi:latest
$ curl http://localhost:5000
# or open http://localhost:5000 with your browser
```

</div>

### Adding Data

In addition to adapting the configuration you will usually add your own data as files or
remote data services like PostGIS or even WFS.

Below, we mount our local directory `data/` to `/pygeoapi/mydata` within the Container.
Within the data directory we may store Vector files like GeoJSON, and even Raster files.
The default configuration [default.config.yml](https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml) is downloaded from the GitHub repository.
We can adapt this file with a local editor, here `vi`, to create *Collections* that reference our
data within `/pygeoapi/mydata`.

Below we also see that the configuration is explictly set to `pygeoapi-config.yml` using both a Docker mount to
`/pygeoapi/pygeoapi-config.xml` (instead of the default `local.config.yml`) and the  
`-e PYGEOAPI_CONFIG=/pygeoapi/pygeoapi-config.yml` (environment) option.

<div class="termy">

```console
$ curl -O https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml
$ vi default.config.yml
$ docker run -p 5000:80 \
    -v $(pwd)/data:/pygeoapi/mydata \
    -v $(pwd)/default.config.yml:/pygeoapi/pygeoapi-config.xml \
    -e PYGEOAPI_CONFIG=/pygeoapi/pygeoapi-config.yml \
    geopython/pygeoapi:latest
$ curl http://localhost:5000
# or open http://localhost:5000 with your browser
```

</div>

More [Docker deployment examples](https://github.com/geopython/pygeoapi/tree/master/docker/examples) can be found in the `pygeoapi` GitHub repository.
