---
title: Exercise 1 - Your first dataset
---

# Exercise 1 - Your first dataset

In this section you are going to publish a vector dataset.

For this exercise, we will use a CSV dataset of [Points of interest in Kosovo](https://github.com/geopython/diving-into-pygeoapi/blob/main/workshop/exercises/data/osm_poi_kosovo.csv),
kindly provided by [Open Street Map Community and GeoFabrik](https://download.geofabrik.de/europe/kosovo.html).

You can find this dataset in `workshop/exercises/data/osm_poi_kosovo.csv`.

This exercise consists of two key steps:

* adapt `workshop/exercises/pygeoapi.config.yml` to define this dataset as an OGC API - Features *collection*
* ensure that pygeoapi can find and connect to the data file

We will use the `workshop/exercises/docker-compose.yml` file provided.

## Verify the existing Docker Compose config

Before making any changes, we will make sure that the initial Docker Compose
setup provided to you is actually working. Two files are relevant:

* `workshop/exercises/docker-compose.yml`
* `workshop/exercises/pygeoapi.config.yml`

To test:

!!! question "Test the workshop configuration"

    1. In a terminal shell navigate to the workshop folder and type:

    <div class="termy">
    ```bash
    cd workshop/exercises
    docker-compose up
    ```
    </div>
    1. Open <http://localhost:5000> in your browser, verify some collections
    1. Close by typing `CTRL-C`

!!! note

    You may also run the Docker container in the background (detached) as follows:

    <div class="termy">
    ```bash
    docker-compose up -d
    docker ls  # verify that the pygeoapi container is running
    # visit http://localhost:5000 in your browser, verify some collections
    docker logs --follow pygeoapi  # view logs
    docker-compose stop
    ```
    </div>

## Publish first dataset

You are now ready to publish your first dataset.

!!! question "Setting up the pygeoapi config file"

    1. Open the file `workshop/exercises/pygeoapi/pygeoapi.config.yml` in your text editor
    1. Look for the commented config section starting with `# START - EXERCISE 1 - Your First Collection`
    1. Uncomment all lines until `# END - EXERCISE 1 - Your First Collection`

Make sure that the indentation aligns (hint: directly under `# START ...`)

The config section reads:

``` {.yml linenums="185"}
poi_kosovo:
    type: collection
    title: Points of Interest Kosovo
    description: The dataset shows the location of the places of interest in Kosovo.
    keywords:
        - places of interest
        - kosovo
    links:
        - type: text/csv
          rel: canonical
          title: data
          href: https://download.geofabrik.de/europe/kosovo.html
          hreflang: AL
    extents:
        spatial:
            bbox: [20,41.9,21.7,43.2]
            crs: http://www.opengis.net/def/crs/OGC/1.3/CRS84
    providers:
        - type: feature
          name: CSV
          data: /data/osm_poi_kosovo.csv
          id_field: osm_id
          geometry:
            x_field: x
            y_field: y
```

The most relevant part is the `providers` section. Here, we define a `CSV Provider`,
pointing the file path to the `/data` directory we will mount (see next) from the local
directory into the Docker container above. Because a CSV is not a spatial file, we explicitly
configure pygeoapi so that the longitude and latitude (x and y) is mapped from the columns `lon`
and `lat` in the CSV file.

!!! Tip

    To learn more about the pygeoapi configuration syntax and conventions see
    the [relevant chapter in the documentation](https://docs.pygeoapi.io/en/latest/configuration.html).

!!! Tip

    pygeoapi includes [numerous data providers](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) which
    enable access to a variety of data formats. Via the OGR/GDAL plugin the number of supported formats is almost limitless.
    Consult the [data provider page](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) how you can set up
    a connection to your dataset of choice. You can always copy a relevant example configuration and place it in the datasets section of
    the pygeoapi configuration file for your future project.

## Making data available in the Docker container

As the Docker container (named `pygeoapi`) cannot directly access files on your
local host system, we will use Docker volume mounts. This can be defined 
in the `docker-compose.yml` file as follows:

!!! question "Configure access to the data"

    1. Open the file `workshop/exercises/docker-compose.yml`
    1. Look for the commented section `# Exercise 1 - `
    1. Uncomment that line  `- ./data:/data`

The relevant lines read:

``` {.yml linenums="43"}
volumes:
    - ./pygeoapi/pygeoapi.config.yml:/pygeoapi/local.config.yml
    - ./data:/data # Exercise 1 - Ready to pull data from here
```

The local `./pygeoapi/pygeoapi.config.yml` file was already mounted. Now
we have also mounted (made available) the entire local directory `./data`.

## Test

!!! question "Start with updated configuration"

    1. Start by typing `docker-compose up` 
    1. Observe logging output
    1. If no errors: open <http://localhost:5000>
    1. Look for the Point of interest collection
    1. Browse through the items of the collection
    1. Check the json representation by adding ?f=json to url (or click 'json' in top right)

## Debugging configuration errors

Incidentally you may run into errors, briefly discussed here:

* A file cannot be found, a typo in the configuration
* The format or structure of the spatial file is not fully supported
* The port (5000) is already taken. Is a previous pygeoapi still running? If you change the port, consider that you also have to update the pygeoapi config file

There are two parameters in the configuration file which help to address these issues. 
Set the logging level to `DEBUG` and indicate a path to a log file. 

!!! tip

    On Docker, set the path of the logfile to the mounted folder, so you can easily access it from your host system. You can also view the console logs from
    your Docker container as follows:

    <div class="termy">
    ```bash
    docker logs --follow pygeoapi
    ```
    </div>

!!! tip

    Errors related to file paths typically happen on initial setup. However, they may also happen at unexpected moments, resulting in a broken service.
    Products such as [GeoHealthCheck](https://geohealthcheck.org) aim to monitor, detect and notify service health and availability. The OGC API - Features
    tests in GeoHealthCheck poll the availability of the service at intervals. Consult the [GeoHealthCheck documentation](https://docs.geohealthcheck.org) for more
    information. 
