---
title: Exercise 1 - Publish your first dataset with pygeoapi
---

# Exercise 1 - Publish your first dataset with pygeoapi

In this section you are going to publish a vector dataset using `pygeoapi`.

We will use the CSV dataset [free-wifi-florence.csv](https://github.com/geopython/diving-into-pygeoapi/blob/main/workshop/docker/data/free-wifi-florence.csv), free WIFI
access points in Florence, kindly provided by [opendata.comune.fi.it](https://opendata.comune.fi.it).
You can find this dataset in the `workshop/docker/data` folder.

This exercise consists of two major steps:

* Adapt the `docker.config.yml` to define this dataset as an OGC API Features *Collection*
* Make sure that pygeoapi can find the data file

We will use the `docker-compose.yml` file provided.

## Verify the existing Docker Compose config

Before making any changes, we will make sure that the initial Docker Compose
setup provided to you is actually working. Two files are relevant:

* `workshop/docker/docker-compose.yml`
* `pygeoapi/docker.pygeoapi.config`

To test:

!!! question "Test the workshop configuration"

    1. In a terminal shell navigate to the workshop folder and type:
    ```console 
    docker-compose up`
    ```
    1. Open `http://localhost:5000` in your browser, verify some collections
    1. Close by typing Control-C

NB you may also run the Docker container in the background (detached):

!!! question "Docker in the background"

    1. Type `docker-compose up -d`
    1. Type `docker ls`; verify that the pygeoapi container is running
    1. open http://localhost:5000 in your browser, verify some collections
    1. view logging: `docker logs --follow pygeoapi`
    1. `docker-compose stop`

## Publish first dataset

You are ready to publish your first dataset.

!!! question "Setting up the pygeoapi config file"

    1. Open the file `workshop/docker/pygeoapi/docker.config.yml` in a text editor.
    1. Look for the commented config section starting with `# START - EXERCISE 1 - Your First Collection`
    1. Uncomment all lines until `# END - EXERCISE 1 - Your First Collection`

Make sure that the indentation aligns (hint: directly under `# START ...)

The config section reads:

``` {.yml linenums="185"}
free_wifi_florence:
    type: collection
    title: Free WIFI Florence
    description: The dataset shows the location of the places in the Municipality of Florence where a free wireless internet connection service (Wifi) is available.
    keywords:
        - wifi
        - florence
    links:
        - type: text/csv
        rel: canonical
        title: data
        href: https://opendata.comune.fi.it/?q=metarepo/datasetinfo&id=fb5b7bac-bcb0-4326-9388-7e3f3d671d71
        hreflang: it-IT
    extents:
        spatial:
            bbox: [11, 43.6, 11.4, 43.9]
            crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
    providers:
        - type: feature
        name: CSV
        data: /data/free-wifi-florence.csv
        id_field: name-it
        geometry:
            x_field: lon
            y_field: lat
```

The most relevant part is the `providers` section. Here we define a `CSV Provider`,
pointing the file path to the `/data` directory we will mount (see next) from the local
dir into the Docker container above. Because a CSV is not a spatial file, we tell `pygeoapi`
that the longitude and latitude (x,y) is mapped from the columns `lon` and `lat`.

!!! Tip

    To learn more about the `pygeoapi` configuration syntax and conventions see
    the [relevant chapter in the documentation](https://docs.pygeoapi.io/en/latest/configuration.html).

!!! Tip

    pygeoapi includes a [number of data providers](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) which enable access to a variety of data formats. Via the OGR/GDAL plugin the number of supported formats is almost limitless.
    Read on the [data provider page](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) how you can set up a connection to your dataset of choice. You can always copy a relevant example configuration and place it in the datasets section of the pygeoapi config file for your future project.

## Making data available in the Docker container

As the Docker container, here named `pygeoapi`, cannot directly access files on your
local host system, we will use `Docker volume mounts`. This can be defined 
in the `docker-compose.yml` file:

!!! question "Configure access to the data"

    1. Open the file `workshop/docker/docker-compose.yml`
    1. Look for the commented section `# Exercise 1 - `
    1. Uncomment that line  `- ./data:/data`

The relevant lines read:

``` {.yml linenums="43"}
volumes:
    - ./pygeoapi/docker.config.yml:/pygeoapi/local.config.yml
    - ./data:/data # Exercise 1 - Ready to pull data from here
```

The local `./pygeoapi/docker.config.yml` file was already mounted. Now
we have also mounted (made available) the entire local directory `./data`.

## Test

!!! question "Start with updated configuration"

    1. Start by typing `docker-compose up` 
    1. Observe logging output
    1. If no errors: open http://localhost:5000
    1. Look for the Free WIFI Collection
    1. Browse through the collection

## Debugging configuration errors

Incidentally you may run into errors, briefly discussed here:

* A file can not be found, a typo in the configuration. 
* The format or structure of the spatial file is not fully supported. 
* The port (5000) is already taken. Is a previous pygeoapi still running? If you change the port, consider that you also have to update the pygeoapi config file.

There are 2 parameters in the config file which help to address these issues. 
You can set the logging level to `DEBUG` and indicate a path to a log file. 

!!! tip

    On docker, set the path of the logfile to the mounted folder, so you can easily access it from your host system. You can also view the console logs from your docker container using `docker logs --follow pygoapi`

!!! tip

    Errors related to file paths likely happen at incidental setup. However they can also happen at unexpected moments, resulting in a broken service. Products like [GeoHealthCheck](https://github.com/geopython/GeoHealthCheck) aim to detect this type of unexpected errors. The OGC APi Features test in GeoHealthCheck polls the availability of the service at intervals.  