---
layout: post
title: "Integration Styles"
description: ""
category: 
tags: ["Enterprise", "Integration","Patterns","Notes"]
---

## Aplication Integration Criteria

- application coupling
- intrusiveness
- technology selection
- data format
- data timeliness
- data or functionality
- remote communication
- reliability

## Aplication Integration Options

**File Transfer** - each application produces files of shared data consumed by the other application

**pro**

- non intrusive. Integrators don't need to know details of other application and don't need to change other application.

**con**

- timeliness, infrequent updates
- systems can get out of sync
- data format not enforced suffiently (semantic dissonance)


**Shared Database** - store shared data in common database

**pro**

- common data format enforced
- wide spread use of databases
- consistent data
- data is always up to date

**con**

- suitable design for shared database when new integrations are required
- possible database performance bottleneck
- performance with distributed applications
- locking conflicts


**Remote Procedure Invocation** - expose procedures that can be invoked remotely

**pro**

- good technology support
- standards (SOAP, XML)
- works with HTTP and can pass firewalls
- applications can provide multiple interfaces to the same data

**con**

- performance
- reliability
- sequencing, doing things in a particular order can make it difficult for systems to change independently
- one application failure incapacitates the other integrated applications, prone to failure

**Messaging** - each application connects to a messagins system to exchange data and invoke behaviour using messages

**pro**

- same decoupling as File Transfer but with frequent updates
- retry mechanism that ensures delivery, storage mechanism hidden unlike Shared Database

**con**

- slower as RPC


