---
title: Exercise 1 - Publish your first dataset with pygeoapi
---

# Exercise 1 - Publish your first dataset with pygeoapi

In this section you are going to publish a vector dataset using `pygeoapi`.

We will use the CSV dataset [free-wifi-florence.csv](https://github.com/geopython/diving-into-pygeoapi/blob/main/workshop/docker/data/free-wifi-florence.csv), free WIFI
access points in Florence, kindly provided by https://opendata.comune.fi.it.
You can find this dataset in the `workshop/docker/data` folder.

This exercise consists of two major steps:

* adapt the `docker.config.yml` to define this dataset as an OAPIF *Collection*
* make sure that pygeoapi can find the data file

We will use the `docker-compose.yml` file provided.

## Verify the existing Docker Compose config

Before making any changes, we will make sure that the initial Docker Compose
setup provided to you is actually working. Two files are relevant:

* `workshop/docker/docker-compose.yml`
* `pygeoapi/docker.pygeoapi.config`

To test:

* type `docker-compose up`
* open http://localhost:5000 in your browser, verify datasets
* close by typing Control-C

NB you may also run the Docker Container in the background (detached):

* type `docker-compose up -d`
* type `docker ls`, verify `pygeoapi` Container is running
* open http://localhost:5000 in your browser, verify datasets
* view logging: `docker logs --follow pygeoapi`
* `docker-compose stop`

## Setting up the pygeoapi config file

* Open the file `workshop/docker/pygeoapi/docker.config.yml` in a text editor.
* Look for the commented config section starting with `# START - EXERCISE 1 - Your First Collection`
* Uncomment all lines until `# END - EXERCISE 1 - Your First Collection`  
* make sure that the indentation aligns (hint: directly under `# START ...)

The config section reads:

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

The most relevant part is the `providers` section. Here we define a *CSV Provider*,
pointing the file path to the `/data` directory we will mount (see next) from the local
dir into the Docker Container above. As a CSV is not a spatial file, we tell `pygeoapi`
that the longitude and latitude (x,y) is mapped from the columns `lon` and `lat`.

!!! Tip

    To learn more about the `pygeoapi` configuration syntax and conventions see
    the [relevant chapter in the documentation](https://docs.pygeoapi.io/en/latest/configuration.html).

!!! Tip

    pygeoapi includes a [number of data providers](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) which enable access to a variety of data formats. Via the OGR/GDAL plugin the number of supported formats is almost limitless.
    Read on the [data provider page](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) how you can set up a connection to your dataset of choice. You can always copy a relevant example configuration and place it in the datasets section of the pygeoapi config file for your future project.

## Making configuration and data available in the Docker Container

As the Docker Container, here named `pygeoapi`, cannot directly access files on your
local host system, we will use *Docker Volume Mounts*. This can be defined 
in the `docker-compose.yml` file:

* open the file `workshop/docker/docker-compose.yml`
* look for the commented section `# Exercise 1 - `
* uncomment that line  `- ./data:/data`

The relevant lines read:

    volumes:
      - ./pygeoapi/docker.config.yml:/pygeoapi/local.config.yml
      - ./data:/data # Exercise 1 - Ready to pull data from here

The local `./pygeoapi/docker.config.yml` file was already mounted. Now
we have also mounted (made available) the entire local directory `./data`.

## Test

Moment of truth!

* start by typing `docker-compose up` 
* observe logging output
* if no errors: open http://localhost:5000
* look for the Free WIFI Collection
* browse through the collection

## Debugging configuration errors

Incidentally you may run into various errors, breifly discussed here:

* A file can not be found, a typo in the configuration, 
* the file format is not fully supported. 

* There are 2 parameters in the config file which help to address these issues. 
You can set the logging level to `DEBUG` and indicate a path to a log file. 

!!! tip

    On docker, set the path of the logfile to the mounted folder, so you can easily access it from your host system. You can also view the console logs from your docker container using `docker logs --follow pygoapi`

!!! tip

    Errors related to file paths likely happen at incidental setup. However they can also happen at unexpected moments, resulting in a broken service. Products like [GeoHealthCheck](https://github.com/geopython/GeoHealthCheck) aim to detect this type of unexpected errors. The OGC APi Features test in GeoHealthCheck polls the availability of the service at intervals.  