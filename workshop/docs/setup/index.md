---
title: Setup of the workshop environment
---

# Setup

There are multiple options to follow the workshop. 
It is important to understand the limitations for each of the options:

- Install `pygeoapi` locally either from sources or the binary package. We do not recommend this option if you are running on windows and/or are not familiar with python. See the installation instructions at ... With a local installation you have full power to adjust the software to your needs, integrate with your favourite python IDE, etc.
- Run `pygeoapi` with Docker locally. You should have Docker Desktop installed on your local machine. You can tweak pygeoapi by mounting a local configuration file and local data into the `pygeoapi` Docker Container. 
- Deploy a Docker image of `pygeoapi` at a cloud provider. We have instructions for cloud provider Hetzner, but other cloud providers provide similar workflows.

We strongly recommend to use Docker (Compose), either locally, with a local VM (like VirtualBox) 
or on a remote (cloud) VM. The version of `pygeoapi` on OSGeo Live may not be the latest as needed for the workshop.

## Install with Pip

*justb4: NOT TRIED YET, may need to install deps(?), GDAL etc*.

<div class="termy">

```console
$ pip install pygeoapi
$ curl -O https://raw.githubusercontent.com/geopython/pygeoapi/master/pygeoapi-config.yml
$ vi pygeoapi-config.yml
$ export PYGEOAPI_CONFIG=pygeoapi-config.yml
$ export PYGEOAPI_OPENAPI=pygeoapi-openapi.yml
$ pygeoapi openapi generate $PYGEOAPI_CONFIG > $PYGEOAPI_OPENAPI
$ pygeoapi serve
$ curl http://localhost:5000
---> 100%
Successfully installed pygeoapi
```

</div>

## Build from Sources

*justb4: NOT TRIED YET, may need to install deps(?), GDAL etc*.

<div class="termy">

```console
$ python -m venv pygeoapi
$ cd pygeoapi
$ . bin/activate
$ git clone https://github.com/geopython/pygeoapi.git
$ cd pygeoapi
$ pip install -r requirements.txt
$ python setup.py install
$ cp pygeoapi-config.yml example-config.yml
$ vi example-config.yml
$ export PYGEOAPI_CONFIG=example-config.yml
$ export PYGEOAPI_OPENAPI=example-openapi.yml
$ pygeoapi openapi generate $PYGEOAPI_CONFIG > $PYGEOAPI_OPENAPI
$ pygeoapi serve
$ curl http://localhost:5000
---> 100%
pygeoapi is up and running
```

</div>

## Install Debian Package

*justb4: is this available?*.


## Using Docker

### Quickstart

Running `pygeoapi` with its built-in config and data is a one-liner with Docker.

<div class="termy">

```console
$ docker run --rm -p 5000:80 geopython/pygeoapi:latest
$ curl http://localhost:5000
# or open http://localhost:5000 with your browser
```

</div>

That's all!

* Runs `pygeoapi` on your local system on port 5000, which is mapped to port 80 inside the Container. 
* Runs with the [default configuration](https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml) and data from the GitHub repo.
* The `--rm` option removes the Docker Container, not the Image, after execution.

Next, you can override the default configuration and add your own data using Docker mounts.

### Custom Configuration

Here we override the default configuration which resides at `/pygeoapi/local.config.yml` within the Container 
with our local file [default.config.yml](https://github.com/geopython/pygeoapi/blob/master/docker/default.config.yml) 
by using Docker Volume Mount (`-v` option).

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
Within the data directory we may store Vector like GeoJSON, and even Raster files.
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

## Deploy with Docker at a Cloud Provider

TO BE SUPPLIED
