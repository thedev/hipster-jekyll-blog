---
layout: post
title: "Architecture Fundamentals"
description: "SDD notes"
category:
tags: ["Notes"]
---
Neal Ford, Mark Richards

*Programmers know the benefits of everything and the tradeoffs of nothing.*

Whitepaper: Who needs an architect by Martin Fowler
*Architecture is about the important stuff whatever it is.*

#Softskills
##Decission making

What is an architecture decision?
implementation/design/architecture

- guide technology decisions

ex:
rest vs components should be distributed remotely

How to identify an architecural decission?

- what is the impact of the decision ?
- does the decision help guide the team to make a technology choice or makes a technology choice for them?

##Justify decission

Groundhog day antipattern: no one undestands why a decission was made and it gets discussed over and over


always debatable

- usage and purpose
- overall message throughput
- internal application coupling
- single point of failure
- performance bottleneck

### publish
- architecture decission
- justification

### documenting 

- email driven architecture - anti pattern
- central location or wiki
- establish early where this can be found (onboarding)
   - can categorize decissions  (general/integration/application)

### Communicating
- critical architecture decission : email, whiteboard with key stakholders

## Anti paterns

**Witches brew:** designed by multiple groups resulting in complex mixture of ideas and no clear vision
Goal: collective knowledge, difficult
Solution: 
- select a mediator, whos role is to resolve the differences (not rule). 
- justification and reasoning for each decission
- on conflict each member must agree with the mediator


**Big bang architecture:** design everything at the beginning when you don't know anything

requirements, technology and business change so must architecture

## Continuous Delivery

engineering practices for software projects

Metrics
- lead time vs cycle time (strive for shorter cycle time)

**Continuous**

Integration: integrate early and often, less pain
Deployment: is always deployed
Delivery: software is always in a deployable state

*Bring pain forward*

Feature branching vs Trunk based development
- F - big scary merge (merge ambush), eager vs late
- T - need to enable cherry picking and untrusted contributors
    - can use feature toggles

branches are used by devs as puzzles when  they are bored at work ???

#Application Architecture

## traditional layered

Concepts:
- closed layers
- separation of concerns
- layers of isolation (control change)

Considerations:
- good starting point (easy to start and refactor)
- watch out for app sinkhole pattern (proxy after proxy after proxy) (20 - 80%)

## event driven

- mediator
- broker

### Mediator topology

Components
- initiating event
- event mediator (what are the steps to process events and propagates events to queues/channels)
  - type (architecture decission)
    - simple mediator spring/mule/camel/service bus
    - apache ode - bpel - (business process execution language)
    - jboss/jbpm - business process manager- (manual aprooval)
- event processors

Concepts
- orchestration
- event processors are completely isolated
- event processor can create another initiating event
- decission early on mediator type
  - this is a mistake, having to choose. you can have both. have a mediator proxy to mule/bpel

### Broker topology

- no mediator
- event processor publishes another event which is picked up from other processors etc.
- reliability is a concern but we have guaranteed delivery
- logic is spread, no central point

large complex => mediator 
smaller, highly decouple evolving => broker

Consideration
- contracts creation, maintainance, versioning
- must address availability in code
- reconnection logic on restart/failure

 
## microkernel

 - core system: minimal functionality required to bring system up (no conditional logic)
 - plugin  (conditional logic)
 - constant change, portion of the application, not all at once

 eg: browser, eclipse, claims processing

Requirements:
- registry - in core system
- contract - strive to make it common (use adapters for different plugins)

When to use
- can be embedded 
- support for evolutionary design
- great pattern for product based applications

## space-based

variable scalability
complex and expensive implementation, typically buy
operations responsability to spin up/scalability

## microservices

- distributed architecture
- light weight broker/ http. no logic in trasport
- completely independent microservices, no shared state, coupling
- standardize on integration but not in how they work
- bounded context
- ideally really really small
- service orchestration can be complex. (brokered style)
- services are responsible for who they call
- can do canary releases and test new services on real users

Goal: maximize easy evolution

Consideration
- great support for cd
- increased uptime during deployment
- contract maintainance can be difficult
- service availability must be addressed
- remote connections must be secured

### consumer drive contract

- executable contract between services
    ex: pacto, pact

Resources

www.devopsbookmarks.com (contract testing, service discovery, etc)


### Traditional SOA

Goal: maximize shared resources

message bus:

- coreography + orchestration
- shared + independent services

## service-based (formal name)

Middle ground between microservices and service oriented architecture

- looser bounded context


#Integration Architecture

TODO: visio templates

DB integration:
- extract/contract pattern to maintain different db schemas


#Enterprise Architecture

What is it?

Company has business strategy and operation model which produces business needs.
Business Operations and IT Systems & Infrastructure are IT capabilities.
How do we align business needs with IT capabilities.

- define architectural principles and standards
- business and IT capabilities model
  - mapping between business model and it capabilities, often not aligned.
- EA governance program, review boards
- business architecture. (what is the business?)
- ETA (enterprise technical architecture) !!! confusion

Straegy - Planning and design (enterprise architect here) - execution (tecnical architect here)

Analogy:
personal goal -  gap - lifestyle, fitness and ability => workout plan (gap analysis) this is enterprise architecture

**Primary goal: facilitate change**
- provide strategic technical and architecture vision and direction
- lead architecture transformation and organizational change
- ensure IT capabilities match business needs and goals
- ensure compliance overall the organization
- *communicate goals, metrics and value across organization* !! important

##From developer to architect

- knowledge triangle
- stuff you know/ you know you don't know/ dont know you don't know (need to find out about things)
- you have to maintain the stuff you know (technical depth)
- technical breadth (stuff you know you don't know) as an architect focus here


# Architect agenda

## Translating requirements
## Architecture tradeoffs
 Story of The Vassa. (failed because of the requirements)

 Dangerous:
 - availability + performance
 - availability + consistency

### Tools

ATAM (architecture tradeoff ...) - doesn't work
why? 
- partenrship and preparation (introduction and roles, agenda, logistics, rules) - problem with all stakeholders in a room
- evaulation phase 1 (presentation skills, present skills, defend architecture through scenarios)
- evaulation phase 2 (prioritize scenarios, defend revised architecture agains scenarios, discuss tradeoffs and present results)
- follow up (finalise architecture based on scenarios, release document including tradeoffs )

waterfall, it will require changes
assumes the application is already completed and not all scenarios ar known upfront

#### What works!
Focus on the goals.
- create architecture presentation (different views)
- validate architecture and establish tradeoffs
- identify and mitigate risk
- get stakeholder buy in (including developers)
- begin with conceptual architecture  and repeat through the process (vet with other architects)
- meet with smaller subsets of stakeholders more frequently

Books:

- Software architecture in Practice
- Software Engineering Institute Digital Library


# Armchair architect - anti pattern

- non coding architects (don't take implementation details in consideration)
- when architects are not involved in full project lifecycle
- when architects have no ideea what they are doing (don't be overly vague)

What to do ?

- stay current and write code
- experience in deev
- don't release architecture decissions to early (need validation)

# Spider web architecture - anti pattern

- using to many webservices

# Applying abstraction

Helps minimize change but comes with a price. (decreased performance, added complexity, increase development effort, increased maintenanc effort)
The extent to which you apply abstraction is based on tradeoffs you arre willing to accept.

Types of abstraction:
- location transparency
- name transparency
- implementation transparency
- access decoupling
- contract decoupling

What level do you need ?

How to do it:
- messaging : location, name, implementation
- adapter : all levels
- ws/rest : location, name, implementation
- ws/soap : location, implementation
- message bus : all levels

# Spider web architecture - anti pattern

- using to many webservices

# Integration hubs, pros & cons

Do we need them?

Problem: 
3 systems that need to communicate. each system talks to other systems in different formats. one system needs to support (HTTP, JMS, FTP ...)

Solution:
Integration hub to abstract different communication formats.

Issue with this solution: (context over integration hub)
Operational complexity:
- deployment pipeline (isolated deployment difficult)
- testing
- all endpoint coupled to hub
- adds friction to engineering practices

=> microservices 
- bounded context (everything service konw is in the context)
- prefer coreography vs orchestration
- need safety net: consumer driven contract
    - tests to enforce contract
- in microservices you often find duplicate functionality
- lego model that allows you to change parts independently but comes at high operational cost
- often end up with hybrids

# Anti patern
volcano and single customer management system for airline ticket
- architect says is bad
- for business it was better if each app had it's own customer management system
- *Architecture: 2d (how it looks) -> 3d (how is operationalized) -> 4d(how it evolves)*
- technical solution is not always best solution for the business
