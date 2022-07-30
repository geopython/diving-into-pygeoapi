---
title: Publish your first dataset with pygeoapi
---

# Publish your first dataset with pygeoapi

In this section you are going to publish a vector dataset using pygeoapi. You can use any dataset or select one of the datasets available in the [/tests/data](https://github.com/geopython/pygeoapi/tree/master/tests/data) folder of the pygeoapi repository.

!!! note

    It is important pygeaopi is able to access the data file. This aspect can be challenging if you are running pygeoapi in a docker environment. We recommend to mount a data folder into the docker container. You are most flexible if you also place the pygeoapi config file in that folder. 

## Setting up the pygeoapi config file

Before actually publishing a dataset, we'll first go through all the steps involved.

Which datasets are published is managed in the pygeoapi configuration file. pygeoapi comes with a [default configuration file](https://github.com/geopython/pygeoapi/blob/master/pygeoapi-config.yml) including some sample data. You can use this configuration file as a starting point. An environment variable `PYGEOAPI_CONFIG` can be used to indicate to pygeoapi the path to the configuration file to be used. This parameter can be set using:

```
export PYGEOAPI_CONFIG=/home/user/workshop-config.yml
```

Then (re)start the pygeoapi service with `pygeoapi serve` and validate the result in a browser, e.g. `http://localhost:5000`

!!! note

    Setting an environment variable in a docker container is possible by adding `-e PYGEOAPI_CONFIG=/home/user/workshop-config.yml` to the docker run statement. Notice that the path should refer to a folder within the container. This can be a mounted folder.

## Dataset providers

pygeoapi includes a [number of data providers](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) which enable access to a variety of data formats. Via the OGR/GDAL plugin the number of supported formats is almost limitless.

Read on the [data provider page](https://docs.pygeoapi.io/en/latest/data-publishing/ogcapi-features.html#providers) how you can set up a connection to your dataset of choice. You can copy the relevant example configuration and place it in the datasets section of the pygeoapi config file. For example for a CSV file, this section is relevant.

``` {.yaml linenums="134"}
providers:
    - type: feature
      name: CSV
      data: tests/data/obs.csv
      id_field: id
      geometry:
          x_field: long
          y_field: lat
```

Save the file, restart the service and check the reult in the brower. The new dataset should now be available in the http://localhost:5000/collections endpoint. Also drill down to the individual items of the collection, to evaluate if the connection works smoothly.

## Debugging configuration errors

Incidentally you may run into configuration errors. A file can not be found, a typo in the configuration, or the file format is not fully supported. There are 2 parameters in the config file which help to address these issues. You can set the logging level to `DEBUG` and indicate a path to a log file. 

!!! tip

    On docker, set the path of the logfile to the mounted folder, so you can easilty access it from your host system. You can also view the console logs from your docker container using `docker logs {container}`

!!! tip

    Errors related to file paths likely happen at incidental setup. However they can also happen at unexpected moments, resulting in a broken service. Products like [GeoHealthCheck](https://github.com/geopython/GeoHealthCheck) aim to detect this type of unexpected errors. The OGC APi Features test in GeoHealthCheck polls the availability of the service at intervals  