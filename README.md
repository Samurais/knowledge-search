# ELK Service

## Start
```
./start-elasticsearch
./start-kibana
```

## Index

### Create Document
```
PUT /twitter/tweet/3 HTTP/1.1
Host: YOUR_IP:9200
Content-Type: application/json

{
    "user" : "kimchy",
    "post_date" : "2016-11-15T14:12:16",
    "message" : " out xxx"
}
```

**twitter** Index
**tweet** Type
**3** type id


### Search Document

```
GET /twitter/tweet/_search HTTP/1.1
Host: YOUR_IP:9200
Accept: application/json
```

```
GET /twitter/tweet/_search?q=message:out HTTP/1.1
Host: YOUR_IP:9200
```

[Search in depth](https://www.elastic.co/guide/en/elasticsearch/guide/current/search-in-depth.html)

[Dealing with Human Language](https://www.elastic.co/guide/en/elasticsearch/guide/current/languages.html#languages)

[Aggregations](https://www.elastic.co/guide/en/elasticsearch/guide/current/aggregations.html)