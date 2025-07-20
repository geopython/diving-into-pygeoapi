---
title: Exercise 1 - Your first dataset
---

# Exercise 1 - Your first dataset

In this section you are going to publish a vector dataset.

For this exercise, we will use a CSV dataset of [Bathing waters in Estonia](https://github.com/geopython/diving-into-pygeoapi/tree/main/workshop/exercises/data/tartu/bathingwater-estonia.csv),
kindly provided by [Estonian Health Board](https://terviseamet.ee).

You can find this dataset in `workshop/exercises/data/tartu/bathingwater-estonia.csv`.

This exercise consists of adjusting `workshop/exercises/pygeoapi.config.yml` to define this dataset as an OGC API - Features *collection*

## Verify the existing Docker Compose config

Before making any changes, we will make sure that the initial Docker Compose
setup provided to you is actually working.

To test:

!!! question "Test the workshop configuration"

    1. In a terminal shell navigate to the workshop folder and type:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker compose up
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        cd workshop/exercises
        docker compose up
        ```
        </div>

    1. Open <http://localhost:5000> in your browser, verify some collections
    1. Close by typing `CTRL-C`

!!! note

    You may also run the Docker container in the background (detached) as follows:

    === "Linux/Mac"

        <div class="termy">
        ```bash
        docker compose up -d
        docker ps  # verify that the pygeoapi container is running
        # visit http://localhost:5000 in your browser, verify some collections
        docker logs --follow pygeoapi  # view logs
        docker compose down --remove-orphans
        ```
        </div>

    === "Windows (PowerShell)"

        <div class="termy">
        ```bash
        docker compose up -d
        docker ps  # verify that the pygeoapi container is running
        # visit http://localhost:5000 in your browser, verify some collections
        docker logs --follow pygeoapi  # view logs
        docker compose down --remove-orphans
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
    Bathing_Water_Estonia:
        type: collection
        title: Bathing Water Estonia
        description: Locations where the Estonian Health Board monitors the bathing water quality
        keywords:
            - bathing water
            - estonia
        links:
            - type: text/csv
              rel: canonical
              title: data
              href: https://avaandmed.eesti.ee/datasets/supluskohad
              hreflang: EE
        extents:
            spatial:
                bbox: [20,57,29,60]
                crs: http://www.opengis.net/def/crs/EPSG/0/4326
        providers:
            - type: feature
              name: CSV
              data: /data/tartu/bathingwater-estonia.csv
              id_field: id
              title_field: Name
              geometry:
                x_field: x
                y_field: y
              storage_crs: http://www.opengis.net/def/crs/EPSG/0/3300
```

The most relevant part is the `providers` section. Here, we define a `CSV Provider`,
pointing the file path to the `/data` directory we will mount (see next) from the local
directory into the Docker container above. Because a CSV is not a spatial file, we explicitly
configure pygeoapi so that the longitude and latitude (x and y) is mapped from the columns `lon`
and `lat` in the CSV file. Notice the `storage_crs` parameter, which indicates the coordinate system which is used in the source data.

!!! Tip

    To learn more about the pygeoapi configuration syntax and conventions see
    the [relevant chapter in the documentation](https://docs.pygeoapi.io/en/latest/configuration.html).

!!! Tip

    pygeoapi includes [numerous data providers](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) which
    enable access to a variety of data formats. Via the OGR/GDAL plugin the number of supported formats is almost limitless.
    Consult the [data provider page](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) how you can set up
    a connection to your dataset of choice. You can always copy a relevant example configuration and place it in the datasets section of
    the pygeoapi configuration file for your future project.

## Test

!!! question "Start with updated configuration"

    1. Start by typing `docker compose up` 
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

    === "Linux/Mac"

        <div class="termy">
        ```bash
        docker logs --follow pygeoapi
        ```
        </div>

    === "Windows (PowerShell)"

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
