---
title: Tiles of geospatial information
---

# Tiles of geospatial information

Change to docker directory:

```
cd WORKSHOP/docker
```


```
docker run -it --rm \
  -v ${PWD}/data:/data \
  emotionalcities/tippecanoe \
tippecanoe --output-to-directory=/data/tiles/ --force --maximum-zoom=16 --drop-densest-as-needed --extend-zooms-if-still-dropping --no-tile-compression /data/shops.geojson
```