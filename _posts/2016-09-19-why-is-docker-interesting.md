---
layout: post
published: false
mathjax: false
featured: false
comments: false
title: Why is Docker interesting ?
tags: docker tools
description: What is Docker and why I think it can help.
headline: ''
modified: ''
categories: ''
imagefeature: ''
---
When I first heard of Docker, it was already popular and it seemed to bring something new and shiny, something about containers and how you can containerize your app.

My initial thought was, there's another buzz word!

I only started looking into this recently and found out it could be quite useful.

The short version is that Docker is a virtual machine without the OS. 
It's build on top of Linux libraries and on Mac and Windows it used Virtual Box.

Analogy:

I am using Parallels on Mac to run Windows apps (let's say Visual Studio) but for this I need a Windows Image. Using Docker I would be able to run Visual Studio, locally,  without the need for the Windows image.

In reality I can't run Visual Studio but I can run .NET core apps, Node js apps, Mongo databases without needing .NET Framework, Mongo ... etc

This is done using containers and images where images represent the definition of a container, and the container is the running instance of an image. 


You can download images from DockerHub or create you own using a dockerfile. The dockerfile is a text file with build instructions, check[this repo](https://github.com/kstaken/dockerfile-examples) for sample files. 

To run Docker you need to install the docker toolbox, which comes with a couple of things. 
- docker-machine (creates docker hosts)
- docker client (CLI for working with images and containers)
- Kitematic UI (UI to build and run containers)

```
docker pull [image-name]
docker images
docker rmi [image-name]
```
Once you get an image you can link your source code by mounting a host directory on the container using [Volumes](https://docs.docker.com/engine/tutorials/dockervolumes/).

```
 docker run -v [path]:[container-path] ... 
```

Once you have an image you can easily start a container.

```
docker run [image-name]
docker ps -a 
docker rm [container-name]
```

You can also link multiple containers by naming containers and linking them by name.

```
docker run -d --name my-mongo mongo
docker run -d -p 5000:5000 --link my-mongo:mongo alex/net-core
```

Or by creating a network. This approach has the advantage of providing isolation and only containers inside the network can communicate.

```
docker network create --driver bridge my-network
docker run -d --net=my-network  --name my-mongodb mongo
```



 

