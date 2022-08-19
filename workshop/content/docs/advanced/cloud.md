---
title: Cloud deployment
---

# Cloud deployment

Deployment to cloud infratructure and concepts such as Microservices and [Twelve-Factor](https://12factor.net) present specific requirements to
how software is designed and implemented. pygeoapi supports these concepts, having a low footprint on CPU and memory, and does not persist user
state, therefore being able to scale without risks.

## pygeoapi and Docker

A [Docker image](https://hub.docker.com/r/geopython/pygeoapi) is available for pygeoapi. You can run the image locally as:

<div class="termy">
```bash
docker run -p 5000:80 https://hub.docker.com/r/geopython/pygeoapi:latest
```
</div>

!!! question "Review the pygeoapi docker file"

    Notice in the [pygeoapi docker file](https://github.com/geopython/pygeoapi/Dockerfile) how the open api file is generated as part of the docker startup script. 

In a typical configuration one would override the default pygeoapi configuration file in the image with a customized one and include the data folder

<div class="termy">
```bash
docker run -p 5000:80 \ 
-v $(pwd)/pygeoapi-config.yml:/pygeoapi/local.config.yml \
-v $(pwd)/geodata:/geodata https://hub.docker.com/r/geopython/pygeoapi:latest
```
</div>

Alternatively, you can build a fresh Docker image including both the configuration and data for the service. 

```
FROM geopython/pygeoapi:latest
COPY ./my.config.yml /pygeoapi/local.config.yml
```

You may have noticed that the pygeoapi configuration file includes a reference to the endpoint on which pygeoapi is published. This configuration should
match the public endpoint of the service (domain, path and port).

By default the pygeoapi Docker Image will run from the `root` path `/`. If you need to run from a sub-path and have all internal URLs correct you can
set the `SCRIPT_NAME` environment variable.

<div class="termy"> 
```bash
docker run -p 5000:80 -e SCRIPT_NAME='/mypygeoapi' \
-v $(pwd)/my.config.yml:/pygeoapi/local.config.yml -it geopython/pygeoapi
# browse to http://localhost:5000/mypygeoapi
```
</div>

# Summary

Congratulations! You can now deploy pygeopi as a cloud native service.
