---
layout: post
title: Basic messaging concepts
description: ""
category: null
tags: "Enterprise,Integration,Patterns,Notes"
published: true
headline: ""
modified: ""
categories: 
  - messaging
  - integration
imagefeature: ""
mathjax: false
featured: false
comments: false
---


## Channels

Transmit data between sender and receiver where one application writes information to the channel and the other receives it.
Channels need to be defined in a new messaging system.

### Channel names

A hierarchical naming convention can be addopted to clarify the role of the channel.

Company/Prod/OrderProcessing/NewOrders

### Types of channels

- point to point
- publish subscribe
- datatype channels
- selective consumer
- invalid message channel

### Message Bus

Well designed set of channels that act as an API to a group of applications


## Messages

Atomic packets of data.
It has a header which describes the data being transmited (origin, destination ...) and a body which contains the data.

When one application needs to send more data than a message can hold it can break it into a *Message Sequence*.

If data is valid only for a limited amount specify the use-by time using *Message Expiration*.

Since all senders and receivers must agree on message format, specify the format as a *Canonical Data Model*.

### Types of messages

- command message
- document message
- event message
- request reply message

## Pipes and filters

Used to perform complex processing on a message while maintaing independence and flexibility by divinding a large processing task into a sequence of smaller processing steps (filters) that are connected by channels(pipes).

### Pipeline processing

Architectural style that allows messages to be processed concurently as they pass through the filters.
Compared to sequencial processing a  pipeline can increase performance.

## Routing

Equivalent of *Mediator* pattern in [GoF]

A *Message Router* is a special filter which consumes a message from a channel and passes it to another channel based on a set of conditions. **predictive routing**

Possible bottleneck if destinations change frequently. 

As alternative we can use a *Publish-Subscribe Channel* and a list of *Message Filters*. **reactive filtering**

### Message router variants

- fixed router (intentionally decouple system and insert another router later)
- content based router (relies on message type or data)
- context based router (environment conditions like load balancing, failover)
- stateless/stateful
- dynamic router (configures itself based on control messages from recipients)
- hardcoded logic
- connected to *Control Bus* to allow rule changes without code changes or message interuptions


The *Message Router* is central to the concept of **Message Broker** which permits participating applications not to be aware of other applications by brokering between them.

## Transformation

Equivalent of *Adapter* pattern in [GoF].

Intermediate filter that reconciles format requirements.

## Endpoints

Provide a way to connect applications to the messaging system.
