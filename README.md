# ELK Service
[elasticsearch](https://hub.docker.com/_/elasticsearch/)
[kibana](https://hub.docker.com/_/kibana/)

## Configuration
```
cd elk-service
cp esconfig/elasticsearch.sample.yml esconfig/elasticsearch.yml
cp esconfig/log4j2.sample.properties esconfig/log4j2.properties
```

Update the Configurations.

## Start
```
cd elk-service
./start-elasticsearch -d
./start-kibana -d
open YOUR_IP:5061
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

## Security
[How to Secure Elasticsearch and Kibana](https://www.mapr.com/blog/how-secure-elasticsearch-and-kibana)

*  /etc/nginx/sites-available/elk-es.xxx.net.conf
```
server {
    listen 80;

    server_name elk-es.xxx.net;
    location / {
        auth_basic           "Protected Elasticsearch";
        auth_basic_user_file /opt/elk/.espasswd;

        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-NginX-Proxy true;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:9200;
        proxy_redirect off;
  }
}
```

*  /etc/nginx/sites-available/elk-kibana.xxx.net.conf
```
server {
    listen 80;

    server_name elk-kibana.xxx.net;
    location / {
        auth_basic           "Protected Kibana";
        auth_basic_user_file /opt/elk/.espasswd;

        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-NginX-Proxy true;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_pass http://127.0.0.1:5601;
        proxy_redirect off;
  }
}
```

## Client
[elasticsearch.js](https://www.elastic.co/guide/en/elasticsearch/client/javascript-api/current/index.html)
[Importing data into Elasticsearch](https://gist.github.com/Samurais/0da7bcbe0cc5830b118b411596f2c171)

## Further Reading
[Elasticsearch Definitive Guide](./elasticsearch-definitive-guide-en.pdf)

## Trouble Shooting
[Fielddata is disabled on text fields by default](https://www.elastic.co/guide/en/elasticsearch/reference/5.0/fielddata.html)
```
PUT /chatbot/_mapping/messageinbound/ HTTP/1.1
Host: elk-es.snaplingo.net
Content-Type: application/json
Authorization: Basic xxxxxxxxxxxxxxx

{
  "properties": {
    "fromUserId": { 
      "type":     "text",
      "fielddata": true
    }
  }
}
```