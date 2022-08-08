---yml
title: Workshop environment:setup
---

# Setup
 
In this workshop we use the following materials:

1. **Documentation** - (like this page): access latest on [dive.pygeoapi.io](https://dive.pygeoapi.io)
1. **Exercises** - [download this zip file](https://github.com/geopython/diving-into-pygeoapi/archive/refs/heads/main.zip), unzip, find exercises under `workshop/Docker` [^1]
1. **Docker** - all examples/exercises are run in a `Docker container` 

[^1]: alternatively you can fork/clone the GitHub repository of this workshop from https://github.com/geopython/diving-into-pygeoapi.

The main requirement for the training is, to install Docker and/with `Docker Compose` on your system.
We strongly advise to install Docker before the workshop starts.

Although several custom installation-methods for `pygeoapi` are available and well-documented 
at [pygeoapi.io](https://pygeoapi.io), these will *not* be considered in this workshop. 
Exercises will also be based on Docker, hence a custom installation would at least be 'challenging'. 
The good news is that only a single installation (Docker) is needed! The Docker images used
will contain the latest `pygeoapi` and all its dependencies and external services like PostGIS.

## About Docker

Docker has been available for almost 10 years, widely used on OSGeo software, so chances are big that you have
heard of Docker and possibly of `containerization` in general. Or, better you are familiar and hopefully using Docker already.
If not, there is a plethora of introductory materials like [here](https://www.ibm.com/in-en/cloud/learn/Docker).
[Docker.com](https://www.Docker.com/) remains the main entry.

Especially for the deployment and integration of Open Source Geospatial components, Docker
is a blessing as it provides consistent packaging, isolation, integration and upgrade patterns, compared
to custom installations. Though today we mainly use Docker, the bigger picture is the use of *Containers* as a next step
in virtualization, but that would deserve its own workshop...

[Docker Compose](https://docs.Docker.com/compose/) is an addition to Docker to facilitate 
the orchestration (configuration) of one or more Docker 'Containers' (a Container is a running instance of a Docker image) 
using a configuration convention, "The Docker Compose YAML File", usually named `Docker-compose.yml`.

Stepping up further are even more sophisticated Docker orchestrators like 
[Rancher](https://rancher.com/products/rancher) and [Kubernetes](https://kubernetes.io/), but for
our workshop `Docker` and `Docker compose` are all we need.

## Installation

Docker installation has greatly progressed over the years. This is the only part of the workshop
which is dependent on the system/OS you are running: Windows, Mac or Linux. For each
system the Docker website provides detailed installation instructions. Please follow these consistently.

!!! Note "Docker compose variants"

    Docker Compose in older (pre Compose v2) versions was a separate (Python) program to install,
    though it was usually present in Docker Desktop. 
    The `Docker compose` command in that case is `Docker-compose` (hyphened).
    Very recent (since 2021) Docker (Desktop)-versions include Compose in the Docker CLI. 
    The command is then `Docker compose` (space).
    
    In our texts we will use `Docker-compose`. Dependent on your installation you may need
    to replace the hyphen (-) with a space. But you can always install the original
    compose, (`Docker-compose`) e.g. with `pip install Docker-compose`.

For many platforms a product called `Docker Desktop` is available, which includes `Docker compose`:

* Windows [installation](https://docs.Docker.com/desktop/install/windows-install/)
* Mac [installation](https://docs.Docker.com/desktop/install/mac-install/)
* Linux [installation](https://docs.Docker.com/desktop/install/linux-install/)

Some notes:

* On Windows we recommend using the [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) (WSL) as it also provides a powerful (Bash) commandline and has optimal integration with Docker.
* On Mac, if you are using [Homebrew](https://brew.sh), consider (as the author has) using the [brew Docker formula](https://formulae.brew.sh/formula/Docker)
* On Linux you choose the relevant installer for your platform. You can also use Virtualbox with a Ubuntu Image or use a cloud VM.
* Docker desktop includes a graphical user interface with some interesting options. You can see logs and information about running containers, open their service in a browser or even open a terminal inside the container.

If all goes well, you should be able to run Docker from the commandline as: [^2] 

```console
docker --version
docker-compose --version  
``` 

[^2]: For recent version of Docker: docker compose version


## Quickstart

Once Docker is available on your system, running the `pygeoapi` container with its built-in configuration and 
data is a one-liner. 

!!! question "First run via Docker"

    Open a terminal session and run:

    ```console
    docker run --rm -p 5000:80 geopython/pygeoapi:latest
    ```

That's all! Open your browser and navigate to `http://localhost:5000`, the pygeoapi page will display.
As part of the initial `Docker run` Docker will download the `pygeoapi` Docker Image from [Docker hub](https://hub.Docker.com/r/geopython/pygeoapi). 
This may take some time, as the Docker image includes all 
dependencies like GDAL etc. Be patient! This is only once for the entire workshop, or
you may want to do this beforehand. 

Some notes:

* Docker runs a `pygeoapi` container on your local system on port 5000, which is mapped to port 80 inside the container. 
* Runs with the [default configuration](https://github.com/geopython/pygeoapi/blob/master/Docker/default.config.yml) and data from the GitHub repo.
* The `--rm` option removes the Docker Container, not the Image, after execution.
* Hit ctrl-c to stop the container and return to the terminal.

Next, you can override the default configuration and add your own data using [Docker volumes](https://docs.Docker.com/storage/volumes/).

## Customizing configuration

In the upcoming exercises we are going to update the configuration file multiple times.
For ease of development we are overriding the `pygeoapi` configuration which resides at `/pygeoapi/local.config.yml` 
within the container by a local file which you can edit in your favourite text editor. 

!!! question "Override the pygeoapi config file"

    Download [default.config.yml](https://raw.githubusercontent.com/geopython/pygeoapi/master/Docker/default.config.yml) to the current folder (or navigate to the folder where you downloaded the file), for example with:

    ```
    wget https://raw.githubusercontent.com/geopython/pygeoapi/master/Docker/default.config.yml
    ```

    Open the file in your favourite text editor and change the title and description of the api.

    ``` {.yml linenums="59"}
    metadata:
        identification:
            title: My first pygeoapi run
            description: pygeoapi provides an API to geospatial data
    ```

    Now run the container with the overridden config file:

    ```console
    $ docker run -p 5000:80 \
        -v $(pwd)/default.config.yml:/pygeoapi/local.config.yml \
        geopython/pygeoapi:latest
    ```

    Navigate to http://localhost:5000 to verify the new title and description.


by using a Docker volume mount (`-v` option). A Docker volume mount attaches, 'mounts', a 
directory or single file from your host/local system into the Docker Container.

In above snippet `$(pwd)` indicates the working folder from which you start the Docker container. 

## Adding data and setting the configuration file

In addition to adapting the configuration you will usually add your own data as files or
remote data services like PostGIS or even WFS.

You can also mount a local directory like `data/` to `/pygeoapi/mydata` within the Container.
Within the data directory you can store Vector files like GeoJSON, Raster files or sets of image of vector tiles.

The below example shows an example where the configuration is explictly set to `pygeoapi-config.yml` via an environment variable (-e) and uses a Docker mount to mount the locat `data` folder as `/pygeoapi/mydata`.

<div class="termy">

```console
$ docker run -p 5000:80 \
    -v $(pwd)/data:/pygeoapi/mydata \
    -v $(pwd)/default.config.yml:/pygeoapi/pygeoapi-config.xml \
    -e PYGEOAPI_CONFIG=/pygeoapi/pygeoapi-config.yml \
    geopython/pygeoapi:latest
```

</div>

In the next sections you'll see more examples of mounts to the data folder. More [Docker deployment examples](https://github.com/geopython/pygeoapi/tree/master/Docker/examples) can be found in the `pygeoapi` GitHub repository.
