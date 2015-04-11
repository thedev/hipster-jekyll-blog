---
layout: post
title: "Enterprise Integration Patterns. Intro"
description: ""
category: notes
tags: []
---
Recently I've been involved in different integrations. 
While doing research the book mentioned in the title came up often and I decided to have a look.
Planning to take notes and a series of posts seems like a good way to do it. So here goes.

## Integration fundamental challenges:

- networks are unreliable
- networks are slow
- applications are different
- change is inevitable


## Approaches to integration:

- file transfer
- shared database
- remote procedure invocation
- messaging


## Messaging

Allows asynchronous communication between applications (producers, consumers) by sending packets (messages) via channels (queues).
A channel acts like an array of messages that is shared accross multiple computers.

## Messaging system

Manages messages the same way DBs manage persistance.
Ensures message delivery. (network not available, applications not available)

### Concepts

*Send and forget* - producers send messages to channels and resume work, ensured that message will be delivered
*Store and forward* - during sending, the messaging systems stores the message on the sender computer and forwards to the queue, process may repeat when message is resent.

## Why use messaging

- remote communication
- platform/language integration
- async communication
- variable timing
- throttling
- reliable communication
- disconected operation
- mediation
- thread management


## Challenges of Async Messaging

- complex programming model
- sequence issues
- synchronious scenarios: eg. airline tickets
- performance: adds overhead to communication

## Async vs sync programming

Most apps use sync function calls and this implies the calling process is halted while the subprocess is executing.
With messaging the caller uses send-and-forget and continues to execute after it sends the message. 

Implications

- no single thread of execution. Multiple processes can increase performance but can also make debugging more difficult.
- if results arrive via callbacks the caller must be able to process results while in the middle of other operations. 
- sub-processes must be able to run independently





