# ELK Service

## Start
```
./start-elasticsearch
./start-kibana
```

## Index
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