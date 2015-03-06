---
layout: post
title: "Web Api 2 notes "
description: ""
category: 
tags: []
---
{% include JB/setup %}

# Web Api 2

For mobile applications HTTP should be used as an application protocol instead of using it just for transport.
Provides flexibility to create and use different formats.

HTTP Frameworks in .NET
- WCF Web HTTP
- WCF Data services (build odata services)
- ASP.NET MVC
- ASP.NET Web Api (specific for creating HTTP services)

Why use web api:
- first class http programming model, same model used on the client (HTTPRequest, HTTPResponse)
- easy way to define resources
- ASP.NET routing 
- Formatters for JSON/XML/Odata... custom formaters
- content negotiation (client specifies format)
- validate request messages (data annotations)
- link generation helpers
- sepparating concerns, auth, caching
- generating help docs
- can be hosted in IIS or outside selfhost
- lightweight, async model

2.1
- attribute routing
- global error handling
- request batching (rich odata support)
- OWIN integration, open web interface for .net, standard for how servers and clients talk. if app is written against owin you can take it and run it agains owin capable backends (ASP.NET)
- custom http routes (constraint, data tokens)
- improved help page to give type info
- 

Response Format
- specified using HTTP
	Accept: application/bson
Adding extra formatters

public static class WebApiConfig
{
	public static void Register(HttpConfiguration config)
	{
		// Web API configuration and services
		config.Formatters.Add(new BsonMediaTypeFormatter());

		// Web API routes
		config.MapHttpAttributeRoutes();

		config.Routes.MapHttpRoute(
				name: "DefaultApi",
				routeTemplate: "api/{controller}/{id}",
				defaults: new { id = RouteParameter.Optional }
				);
	}
}





