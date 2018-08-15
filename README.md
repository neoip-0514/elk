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

## 참조

``` http
http://jtoday.tistory.com/51
http://brownbears.tistory.com/66
https://hiseon.me/2018/02/19/install-docker/
https://www.codementor.io/samueljames/using-docker-with-elasticsearch-logstash-and-kibana-elk-dzucc3c94
http://elk-docker.readthedocs.io/#running-with-docker-compose
```
