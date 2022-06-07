---
title: Setup of the workshop environment
---

# Setup

There are multiple options to follow the workshop. It is important to understand the limitations of each of the options:

- Install locally either from sources or the binary package. We do not recommend this option if you are running on windows and/or are not familiar with python. See the installation instructions at ... With a local installation you have full power to adjust the software to your needs, integrate with your favourite python IDE, etc.
- Run pyeoapi on docker locally. You should have docker desktop installed on your local machine. You can tweak pygeoapi by mounting it's configuration files from your local system into the docker container. 
- Deploy a docker image of pygeoapi at a cloud provider. We have instruction for cloud provider xxx, but other cloud providers provide similar workflows.

## Deploy with a package manager

```
pip install pygeoapi
wget https://raw.githubusercontent.com/geopython/pygeoapi/master/pygeoapi-config.yml
vi pygeoapi-config.yml
export PYGEOAPI_CONFIG=pygeoapi-config.yml
export PYGEOAPI_OPENAPI=pygeoapi-openapi.yml
pygeoapi openapi generate $PYGEOAPI_CONFIG > $PYGEOAPI_OPENAPI
pygeoapi serve
curl http://localhost:5000
```

## Build from sources

```
python -m venv pygeoapi
cd pygeoapi
. bin/activate
git clone https://github.com/geopython/pygeoapi.git
cd pygeoapi
pip install -r requirements.txt
python setup.py install
cp pygeoapi-config.yml example-config.yml
vi example-config.yml
export PYGEOAPI_CONFIG=example-config.yml
export PYGEOAPI_OPENAPI=example-openapi.yml
pygeoapi openapi generate $PYGEOAPI_CONFIG > $PYGEOAPI_OPENAPI
pygeoapi serve
curl http://localhost:5000
```

## Deploy as docker container

```
wget https://raw.githubusercontent.com/geopython/pygeoapi/master/pygeoapi-config.yml
vi pygeoapi-config.yml
docker run -p 5000:5000 \
    -v {$pwd}/pygeoapi-config.yml:/pygeoapi/pygeoapi-config.xml \
    -v {$pwd}/data:/pygeoapi/data \
    -e PYGEOAPI_CONFIG=/pygeoapi/pygeoapi-config.yml \
    geopython:pygeoapi:latest
```

## Deploy docker container at a cloud provider