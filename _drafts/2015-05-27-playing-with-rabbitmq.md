---
layout: post
title: "Hands on with RabbitMQ"
description: ""
category: ["notes"]
tags: ["notes", "messaging"]
---


# General notes
Going through the RabbitMQ docs (.NET) and taking some notes.

After instalation broker needs to be started:

C:\Program Files (x86)\RabbitMQ Server\rabbitmq_server-3.5.1\sbin>rabbitmq-server.bat


To see what's going on I installed the Management plugin (https://www.rabbitmq.com/management.html) which provides a management interface accessible on:

http://localhost:15672/#/


##Work/Task Queue

Used to distribute tasks between multiple workers.
Round robin dispatching
Message acknowledgement
Message durability
Fair dispatch


###Exchanges

A producer never send messages to queues in RabbitMQ, it sends them to an Exchange and the exchange sends the messages to a queue (bindings).

Types of exchanges:
 - direct
 - topic 
 - headers
 - fanout

rabbitmqctl list_exchanges 

####Direct exchanges

Message goes to the queue whose binding key exactly matches the routing key of the message.

####Topic Exchange

Direct exchanges and bindings does not allow us to do routing based on multiple criteria.

###Temporary queues

###Bindings

The binding is the relationship between the exchange and the queue.

channel.QueueBind(queueName, "logs", "");

rabbitmqctl list_bindings

Bindings can take an extra routingKey parameter:
channel.QueueBind(queueName, "direct_logs", "black");



# Demo

WEB api that pushes messages to rabbitmq.
Consumers: 
- filter - validates the message and sends the message in the processing pipeline
- consumer1 - c1 logic and based on the result moves the message to c2 or success queue
- consumer2 - c2 logic and moves message to success queue or fail queue

Instead of using a fanout approach I want to get the message from c1 to c2 to better understand how the routing works.

Management:
- have a look at Smart Proxy or a Control Bus








 
