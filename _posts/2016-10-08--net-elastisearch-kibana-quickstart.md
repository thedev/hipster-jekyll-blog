---
layout: post
published: true
mathjax: false
featured: false
comments: true
title: Elastisearch & Kibana Quickstart
categories:
  - engineering
  - webdevelopment
tags: 'tools, docker, kibana, elasticsearch'
---
The ELK stack (Elasticsearch, Logstash, Kibana) is becoming more popular as a solution for centralised logging due to it's simplicity and powerfull features. It's easy to set up, integrate and use.

**Setup**

Locally, instead of installing elasticsearch and kibana we can use docker, this enables us to easily spin up and tear down both of these services without the need to install them.

Docker is also a good candidate for production but there are out of the box solutions wich deal with a lot of the complexity for you, like managing the Elasticsearch cluster. see [AWS ElasticSearch as a service](https://aws.amazon.com/elasticsearch-service/).

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
If you are using a regular terminal you will need to set some environment variables by running `eval $(docker-machine env)`.

`docker-machine start default`

Running docker-compose up will download the images and start the containers. At this point both elastic search and kibana should be avaiable.

	Kibana: http://192.168.99.100:5601/ 
	Elasticsearch: http://192.168.99.100:9200/

_TIP:_ 

ElasticSearch provides a REST Api which can be very handy.
I'm using Postman and a saved custom collection with useful requests, as an example the follwing request will display a list of indices.

`http://192.168.99.100:9200/_cat/indices?v`


**NLog**

Install NLog using NuGet: `Install-Package NLog.Config`

NLog provides a [custom elsticsearch target](https://github.com/ReactiveMarkets/NLog.Targets.ElasticSearch) which behind the scenes uses [Elasticsearch-net](https://github.com/elastic/elasticsearch-net) as a low level client.

Install NLog custom target:  `Install-Package NLog.Targets.ElasticSearch`

This setup allows you to try Kibana and Elasticsearch and will get you thinking about the next steps.

- What is the logging strategy ? Should we use content indetifiers?
- How do we handle elasticsearch cluster failover?
- What type of dashboards would help?
- Does it make sense to add Logstash into the equation ?
