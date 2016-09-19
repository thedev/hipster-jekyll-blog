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
I think I've first heard about Docker sometime last year. It sounded like something big, but very unclear. Something about containers and how you can containerize your app.

My initial thought was, there's another buzz word!

At that moment I was focusing on something else but recently I re-enabled my Pluralsight subscription and saw a Docker for Web Developer course, by Dan Wahlin. I went through the course and things started to make sense.

The short version is that Docker is sort of a virtual machine, but lighter, without the OS. 
It's build on top of Linux libraries and on Mac and Windows it used Virtual Box.

I am using Parallels on Mac to run Windows apps (let's say Visual Studio) but for this I need a Windows Image. Using Docker I would be able to run Visual Studio, locally,  without the need for the Windows image.

In reality I can't run Visual Studio but I can run .NET core apps, Node js apps, Mongo databases without needing .NET Framework, Mongo ... etc

This is done using containers and images.
Images represent the definition of a container, and the container is the running instance of an image. 


You can download images from DockerHub or create you own using a dockerfile which is a text file with build instructions.

Here are a few sample dockerfiles: https://github.com/kstaken/dockerfile-examples

To run Docker you need to install the docker toolbox, which comes with a few tools. 
- docker-machine (creates docker hosts)
- docker client (CLI for working with images and containers)
- Kitematic UI (UI to build and run containers)



