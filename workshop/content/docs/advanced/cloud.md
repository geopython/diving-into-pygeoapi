---
title: Cloud deployment
---

# Cloud deployment

Deployment in cloud infratructure presents specific requirements to software. pygeoapi has a low footprint on cpu and memory, does not persist user state and can therefore scale without risks.

## pygeoapi and docker

A [docker image](https://hub.docker.com/r/geopython/pygeoapi) is available for pygeoapi. You can run the image locally as:

```
docker run -p 5000:80 https://hub.docker.com/r/geopython/pygeoapi:latest
```

!!! question "Review the pygeoapi docker file"

    Notice in the [pygeoapi docker file](https://github.com/geopython/pygeoapi/Dockerfile) how the open api file is generated as part of the docker startup script. 

In a typical configuration one would override the default pygeoapi configuration file in the image with a customised one and include the data folder

```
docker run -p 5000:80\ 
  -v $(pwd)/pygeoapi-config.yml:/pygeoapi/local.config.yml\ 
  -v $(pwd)/geodata:/geodata https://hub.docker.com/r/geopython/pygeoapi:latest
```

Alternatively you can build a fresh docker image including the config and data for the service. 

```
FROM geopython/pygeoapi:latest
COPY ./my.config.yml /pygeoapi/local.config.yml
```

You may have noticed that the pygeoapi configuration file includes a reference to the endpoint on which pygeoapi is published. This configuration should match the public endpoint of the service (domain, path and port). 

By default the `pygeoapi` Docker Image will run from the `root` path `/`. If you need to run from a sub-path and have all internal URLs correct you need to set `SCRIPT_NAME` environment variable.

```
docker run -p 5000:80 -e SCRIPT_NAME='/mypygeoapi' 
    -v $(pwd)/my.config.yml:/pygeoapi/local.config.yml -it geopython/pygeoapi
# browse to http://localhost:5000/mypygeoapi
```


