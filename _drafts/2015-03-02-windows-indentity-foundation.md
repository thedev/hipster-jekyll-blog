---
layout: post
title: "Windows Identity Foundation"
description: ""
category: 
tags: []
---
{% include JB/setup %}


Why claims based authentication?

Allows you to delegate authentication to another entity.

WIF contains a set of components that allow devs to externalize identity logic using a single simplified model based on claims and WS- protocols.

Claims can also provide additional information that may be required beside the user/pass check.

Terminology

subject/principal = user
IP - identity provider = token issuer that holds auth logic
RP - relying party = application/web service
claim = statements about authenticated user that allow the RP to perform authorization
token = envelope for claims (simple user/pass combination, SAML, binary based)
STS - secure token service = web service exposed by IP that provides the auth service


WS-* standards 

WS-Security
SOAP extension that adds authentication/authorization, message protection and integrity to messages
Authentication/Authorization is implemented using security tokens, claims carried inside token  aid in authorizaiton.
Message protection is done using SSL at transport level and XML Signatures at message level.
Message integrity is also checked through XML Signatures.

WS-Policy
Specifies RP - IP interaction security policies.

WS-Addressing (previously WS-Routing)
Provides end to end transport independent message transmission.
Supports implementation of message routing and allows you to specify a different response or fault return URL.

WS-Trust
Allows delegation of auth to other IP.
Provides STS.
Defines a message request called RequestSecurityToken(RST) issued to STS. STS replies via a response called RequestSecuirtyTokenResponse (RSTR) that holds the security token to be used to grant access.
WS-Trust describes the protocol for requesting tokens vis RST and issuing tokens via RSTR.


WS-SecureConversation
Use to improve service response time 
WS-Federation





