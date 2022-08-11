# Environment

## Docker compose environment

### Start
To start Docker Compose, from `workshop/exercises`, run the following command:

```bash
docker-compose up -d
```

This will start two Docker containers, one with our pygeoapi (reachable from the browser at [this address](http://localhost:5000/)) and one with Elasticsearch (localhost:9200), which can be used as storage backend.

To stop, without deleting the data stored on Elasticsearch, use the command:

```bash
docker-compose down
```

To stop and delete the data stored on Elasticsearch, use the command:

```bash
docker-compose down -v
```

### Push data in Elasticsearch

Elasticsearch can be reached at `http://localhost:9200`. But to make it easier to load the data you can run the following script:

```bash
docker exec elastic /add_data.sh /path/myfile.geojson geojson_id
```

Which will put the data into a new Elasticsearch index named as the file (*myfile*), identified by the id `geojson_id`


### Troubleshooting

The Elasticsearch container may not work on some Linux distributions. The following command may help fix deployment errors:

```bash
sudo sysctl -w vm.max_map_count=262144
```
