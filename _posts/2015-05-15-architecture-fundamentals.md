---
layout: post
published: false
title: Architecture Fundamentals
description: SDD notes
category: null
tags: Notes
headline: ""
modified: ""
categories: null
imagefeature: ""
mathjax: false
featured: false
comments: false
---

Notes from a talk given by Neal Ford and Mark Richards during SDD London 2015.

*Programmers know the benefits of everything and the tradeoffs of nothing.*

*Architecture is about the important stuff whatever it is.*

#Resources
[Who needs an architect](http://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf) by Martin Fowler

#Softskills
##Decission making

What is an architecture decision and how to identify an architecural decission
- what is the impact of the decision ?
- does the decision help guide the team to make a technology choice or makes a technology choice for them?

##Justify decission

Antipattern: Groundhog day
No one undestands why a decission was made and it gets discussed over and over

Things to consider:
- usage and purpose
- overall message throughput
- internal application coupling
- single point of failure
- performance bottleneck

Important: The decision is always debatable

### Document decission

Antipattern: Email driven architecture

- central location or wiki
- establish early where this can be found (onboarding)
   - can categorize decissions  (general/integration/application)

### Publish decission
- architecture decission
- justification

### Communicate decission
If the architecture decission is critical you can have a whiteboard session with key stakholders.

## Other Antipatterns

**Witches brew:** 
designed by multiple groups resulting in complex mixture of ideas and no clear vision
Goal: use collective knowledge but it can be difficult to get people on the same page
Solution: 
- select a mediator, whos role is to resolve the differences (not rule). 
- justification and reasoning for each decission
- on conflict each member must agree with the mediator


**Big bang architecture:** 
design everything at the beginning when you don't know anything

Why is this not good?
Requirements, technology and business change, so must architecture.


**Continuous**

The term _Conitinuous_ is used a lot and often missunderstood. 

Integration: integrate early and often
Deployment: is always deployed
Delivery: software is always in a deployable state

Why use a Continuous approach?  

*Bring pain forward* - smaller changes will be easier to manage
