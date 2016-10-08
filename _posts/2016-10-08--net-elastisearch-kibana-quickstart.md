---
layout: post
published: false
mathjax: false
featured: false
comments: false
title: '.NET, Elastisearch & Kibana Quickstart'
---
The ELK stack is becoming more popular as a solution for centralised logging due to it's simplicity and features. It's easy to set up, integrate and use.

**Setup**

Locally, instead of installing elasticsearch and kibana we can use docker, this enables us to easily spin up and tear down both of these services without the need to install them.

Docker is also a good candidate for production but there are out of the box solutions. see [AWS ElasticSearch as a service](https://aws.amazon.com/elasticsearch-service/).

A minimal local setup can be obtained using docker compose:

```
version: "2.0"
services:
  elasticsearch:
    image: elasticsearch
    ports:
      - 9200:9200
  kibana:
    image: kibana
    ports:
      - 5601:5601
    environment:
      - ELASTICSEARCH_URL=http://elasticsearch:9200
```

Please note specified ports.
Once this is in place open the Docker Quickstart Terminal in the docker_compose.yml location and run:

`docker-compose up`

_Note:_ 

Running the Quickstart terminal will start the default docker machine and give it an IP, this will be used to access ES and Kibana.
If you are using a regular terminal you will need to set some environment vars and start the docker-machine manually.

`docker-machine start default`

Running docker-compose up will download the images and start the containers. At this point both elastic search and kibana should be avaiable.

	Kibana: http://192.168.99.100:5601/ 
	Elasticsearch: http://192.168.99.100:9200/

_TIP:_ 

ElasticSearch provides a REST Api which can be very handy.
I'm using Postman and saved custom collection with useful requests.


**NLog**

Install NLog using NuGet: `Install-Package NLog.Config`

NLog provides a [custom elsticsearch target](https://github.com/ReactiveMarkets/NLog.Targets.ElasticSearch) which behind the scenes uses [Elasticsearch-net](https://github.com/elastic/elasticsearch-net) as a low level client.

Install NLog custom target:  `Install-Package NLog.Targets.ElasticSearch`

