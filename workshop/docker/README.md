# Environment

## Docker compose environment

### Start
To start the docker composition, from ./workshop/docker folder, use the command:

```bash
docker-compose up -d
```

This will start two docker containers, one with our pygeoapi (reachable from the browser at this address) and one with Elasticsearch, which can be used as storage backend.

To stop, without deleting the data stored on Elasticsearch, use the command:

```bash
docker-compose down
```

To stop and delete the data stored on Elasticsearch, use the command:

```bash
docker-compose down -v
```

### Push data in Elasticsearch

Elasticsearch can be reached at localhost: 9200. But to make it easier to load the data you can use the following script:

```bash
docker exec elastic /add_data.sh /path/myfile.geojson geojson_id
```

Which will put the data into a new Elasticsearch index named as the file (*myfile*), identified by the id *geojson_id*


### Troubleshooting

Elasticsearch container may not work on some linux systems. The following command could unblock the situation:

```bash
sudo sysctl -w vm.max_map_count=262144
```




