---
title: Docker Elasticsearch
tags: 
- Docker 
- Elasticsearch
date: 2023-07-05 09:31:06
cover:
---

```
docker run -d --name es01 \
 -p 9200:9200 \
 -e ES_JAVA_OPTS="-Xms1g -Xmx1g" \
 -e "discovery.type=single-node" \
 --ulimit nofile=1024:1024 \
 elasticsearch:8.7.0


docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-reset-password



docker cp es01:/usr/share/elasticsearch/config/certs/http_ca.crt .




curl --cacert http_ca.crt -u elastic https://localhost:9200




```
```
version: "3.9"
services:
  elasticsearch:
    image: elasticsearch:8.6.0
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms1g -Xmx1g
      - xpack.security.enabled=false
    volumes:
      - es_data:/usr/share/elasticsearch/data
    ports:
      - target: 9200
        published: 9200
    networks:
      - elastic
  kibana:
    image: kibana:8.6.0
    ports:
      - target: 5601
        published: 5601
    depends_on:
      - elasticsearch
    networks:
      - elastic      
volumes:
  es_data:
    driver: local
networks:
  elastic:
    name: elastic
    driver: bridge

```
## 8.10.3
docker network create elastic

docker container rm es-1

docker run --name es-1 --net elastic -p 9200:9200 -it -e ES_JAVA_OPTS="-Xms1g -Xmx4g" docker.elastic.co/elasticsearch/elasticsearch:8.7

docker exec -it es-1 bin/elasticsearch-setup-passwords auto

docker exec -it es-1 /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
GBPoTYCI3HxlkFP2o51F

curl -u elastic:021009 https://localhost:9200