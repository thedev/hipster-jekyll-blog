---
layout: post
title: "EIP Solving integration using patterns"
description: ""
category: 
tags: ["Enterprise", "Integration","Patterns","Notes"]
---

## Need for integration

No solution can support all the business functions required by a typical enterprise.

Spreading business functions across applications provides flexibility to choose best available option.

## Integration challenges

>The term simple integration is pretty much an oxymoron.

* shift in corporate politics

  In an integrated enterprise application groups no longer control specific applications because these are part of an overall flow of integrated apps and services.

* far reaching implications on business

  Failing integration solution can have large impact on business.

* limited control over participating applications

  integration developers may need to make up for deficiencies in apps

* lack of interoperabilities between "standard compliant" products
* resolving semantic differences between systems 

  Existence of common presentation (XML) does not solve semantics

* running and maintaing integrated solution

  technologies and distributed nature make deployment, monitoring and debugging complex

## Types of integration

- Information portal

  Display data from different systems.

- Data replication

  Access to same data. Customer Address required by accounting, shipping and billing systems.

- Shared business function

  Redundant functionality. (credit card validation)

- SOA

  Key concepts: service dicovery and negotioation

  [Keynote: Microservices by Martin Fowler](https://www.youtube.com/watch?v=wgdBVIX9ifA)
  

- Distributed business process

  One business process can touch multiple systems. Coordination between different applications is required.

- B2B integration

  Business functions provided by outside suppliers or partners.


## Loose Coupling

*buzz word alert*

Core principle is to reduce assumptions that two parties make about each other.

More assumptions between parties means efficient communication but the integration becomes less tolerant of interruptions or changes.

Tight coupling example: method call (same process, same language, same no. of parameters)

Many integrations aim to make remote communication simple by packaging a remote data exchange into same semantics as a local method call. This is called Remote Procedure Call (RPC) or Remote Method Invocation(RMI).

Looking at remote comunication (RPC, RMI) as a variant of local communication can cause problems because remote invocation brings additional problems. (perforamance, interruptions, security)


### Integration scenario

Web app needs to connects to financial backend.

#### Tightly coupled 

Fast and cheap way to do it: send data through TCP/IP

Issues:

- platform technology: internal representation of numbers and objects
- location: hardcoded machine addresses
- time: all components need to be available at the same time
- data format

#### Loosely coupled
- use XML instead of sending data
- remove location dependency by using a communication channel which is not aware of sender/receiver identity
- channel can queue request and the sender/receiver do not have to be available at the same time
- provide data tranformations inside the channel so that if one system changes we only need to change the transformer for that system


## A loosely coupled integration solution

- channel: moves information
- message: common data
- translation: app format to message
- routing: send message to right place
- systems management: what happens in the system
- message endpoint/channel adapter: connect systems to integration solution
