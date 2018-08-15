# ELK 설치

## 엘라스틱서치 설치

> run docker elasticsearch

``` bash
docker run -d -p 9200:9200 --name es elasticsearch
-- or --
docker run --name es \
    -p 9200:9200 -p 9300:9300 \
    -e "discovery.type=single-node" \
    docker.elastic.co/elasticsearch/elasticsearch:6.3.2
```

- 클러스터링 확인

``` bash
curl -XGET http://localhost:9200/_cluster/health?pretty=true
```

- 웹 확인

``` bash
curl -XGET http://localhost:9200
```

## Logstash

> run docker logstash


``` bash
docker pull docker.elastic.co/logstash/logstash:6.3.2

docker run --rm -it -v ~/pipeline/:/usr/share/logstash/pipeline/ docker.elastic.co/logstash/logstash:6.3.2

docker run --rm -it \
  -v ~/settings/:/usr/share/logstash/config/ \ docker.elastic.co/logstash/logstash:6.3.2

```

## 키바나 설치

> run docker kibana

``` bash
vi kibana-compose.yml
```

``` yml
version: '3.4'
services:
  kibana:
    container_name: kibana
    image: docker.elastic.co/kibana/kibana-oss:6.3.2
    environment:
      ELASTICSEARCH_URL: 'http://172.17.0.1:9200' # docker ip
    ports:
      - '5601:5601'
```

``` bash
docker-compose --file kibana-compose.yml up -d
```

- 확인

``` http
http://localhost:5601
```

## Beats

``` bash
docker run \
  --mount type=bind,source="$(pwd)"/filebeat.yml,target=/usr/share/filebeat/filebeat.yml \
  docker.elastic.co/beats/filebeat:6.3.2
```

## 참조

http://jtoday.tistory.com/51

http://brownbears.tistory.com/66

https://hiseon.me/2018/02/19/install-docker/

https://www.codementor.io/samueljames/using-docker-with-elasticsearch-logstash-and-kibana-elk-dzucc3c94

http://elk-docker.readthedocs.io/#running-with-docker-compose

https://www.elastic.co/guide/en/beats/filebeat/current/running-on-docker.html

https://medium.com/@bcoste/powerful-logging-with-docker-filebeat-and-elasticsearch-8ad021aecd87
