---
layout: post
published: true
title: Architecture Fundamentals
description: SDD notes
category: null
tags: Notes
headline: ""
modified: ""
categories: 
  - personal
imagefeature: ""
mathjax: false
featured: true
comments: true
---


Notes from a talk given by Neal Ford and Mark Richards during SDD London 2015.

*Programmers know the benefits of everything and the tradeoffs of nothing.*

*Architecture is about the important stuff whatever it is.*

#Softskills

##Decission making

In order to be effective the architect must choose when to get involved and when to trust the team. A good way to identify times to get involved is to look at the impact of the decision.

A decision with a big impact should involve the architect. Who has to be careful that the decission he makes will help the team make a technology choice, and not make the choice for them.

##Justify

Try to avoid the _Groundhog day_ antipattern where no one undestands why a decission was made and it gets discussed over and over.

When justifying a decision consider the follwing:

- usage and purpose
- overall message throughput
- internal application coupling
- single point of failure
- performance bottleneck

Important: _The decision is always debatable_

### Document

Try to avoid the _Email driven architecture_ antipattern.

Considerations:

- central location or wiki
- establish early where this can be found (onboarding)
   - can categorize decissions  (general/integration/application)

### Publish

- architecture decission
- justification

### Communicate

After the decision is published, it should be comunicated and for critical decisions a whiteboard session with key stakholders can be ca good idea.

## Other Antipatterns

**Witches brew:** 

Designed by multiple groups resulting in complex mixture of ideas and no clear vision. The goal is to use collective knowledge but it can be difficult to get people on the same page.

A possible solution could be to select a mediator, whos role is to resolve the differences (not rule). Participants must provide justification and reasoning for each decision and on conflict each member must agree with the mediator.


**Big bang architecture:** 

Design everything at the beginning when you don't know anything.
This is bad because requirements, technology and business change, so must architecture.


**Continuous**

The term _Conitinuous_ is used a lot and often missunderstood.

Integration: integrate early and often
Deployment: software is always deployed
Delivery: software is always in a deployable state

By using a continuous approach you *Bring the pain forward* and end up with an process that is easier to manage.

#Interesting read

[Who needs an architect](http://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf) by Martin Fowler
