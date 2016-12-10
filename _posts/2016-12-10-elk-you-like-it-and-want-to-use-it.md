---
layout: post
published: true
mathjax: false
featured: true
comments: true
title: 'ELK - you like it and want to use it ? '
description: Some thoughts on using ELK in a real application
tags: 'elk, scalability'
categories:
  - webdevelopment
  - Devops
---
Starting out with ELK is pretty straightforward and it usually highlights the benefits of a centralised loggic solution. Once you see this you will want to consider a few of the implications of running ELK in a scalable production environment. Here are some of my finding on this.


## We can forward logs directly to elasticsearch

I used NLog and a custom elasticsearch target to get things going. This allows you to quickly view and explore the logs with Kibana. NLog can be substituted with Log4Net or you can interact directly with ES through the Elasticsearch.net or even write your own client.
In this scenario the application is responsible of sending logs directly to Elasticsearch and we have coupling between the application and the ES cluser. 


### Will the ES cluster availability impact application performance?

It depends on the actual implementation for the log sending, this must be non-blocking.
The NLog target seems to handle this well, it will not block your application when it can not deliver logs, provides configuration for exception management and a failover mechanism.

Here is a sample NLog.config using an Elasticsearch target.

```

<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="nlog"
             type="NLog.Config.ConfigSectionHandler, NLog"/>
  </configSections>
  <nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.nlog-project.org/schemas/NLog.xsd NLog.xsd"
        autoReload="true"
        throwExceptions="false"
        internalLogLevel="Off"
        internalLogFile="c:\temp\nlog-internal.log">

    <targets>
      <target name="elastic"
              xsi:type="BufferingWrapper"
              flushTimeout="5000">
        <target xsi:type="ElasticSearch"
                layout="${logger} | ${threadid} | ${message}"
                index="logstash-${date:format=yyyy.MM.dd}"
                includeAllProperties="true"
                uri="ES-URL-HERE">

          <field name="user"
                 layout="${windows-identity:userName=True:domain=False}"/>
          <field name="host"
                 layout="${machinename}"/>
        </target>
      </target>
    </targets>
    <rules>
      <logger name="*"
              minlevel="Debug"
              writeTo="elastic" />
    </rules>
  </nlog>
</configuration>

```

To make sure the target doesn't break the application make sure you

- Use the latest stable version of NLog (4.3)
- Don't enable throwExceptions (disabled by default)
- If you enable async, the errors are written to the target in another thread, so it could not break your app.
- Also when using async, check the overflow and queue settings

You can also take a look at a helpful answer on my SO question:
http://stackoverflow.com/questions/40807162/application-logging-with-elk-stack/40810299#40810299



### Data integrity

Beside application perfromance, another aspect is data integrity, more exactly not loosing logs when ES is down.
With NLog you can prevent this by using an additional target to store logs in a different or a  [FallBackGroup target](https://github.com/nlog/NLog/wiki/FallbackGroup-target).




### Data quality

If you want to take advantage the visualisations Kibana provides you will need to structure and process the logs you send. 
With applicaton logs, where you have control, this needs to be handled by having a well defined logging strategy and structured data.
For logs where you don't have control, the default option with ELK for log processing is Logstash.

## Why use Logstash

So it helps with logs processing, but what exactly does that mean ?
For example, an ELKB log entry is not ideal for data analysis, it's just a long text message which needs to be broken down in different parts. This is where Logstash filters come in.

The `grok` filter below will parse the ELB log into a more usefull document.

```                                                                        

input {
  tcp {
    port => 5000
  }
}

## Add your filters / logstash plugins configuration here

filter {
  grok {
    match => [ "message", "%{TIMESTAMP_ISO8601:timestamp} %{NOTSPACE:elb} %{IP:clientip}:%{INT:clientport:int} (?:(%{IP:backendip}:?:%{INT:backendport:int})|-) %{NUMBER:request_processing_time:float} %{NUMBER:backend_processing_time:float} %{NUMBER:response_processing_time:float} (?:-|%{INT:elb_status_code:int}) (?:-|%{INT:backend_status_code:int}) %{INT:received_bytes:int} %{INT:sent_bytes:int} \"%{ELB_REQUEST_LINE}\" \"(?:-|%{DATA:user_agent})\" (?:-|%{NOTSPACE:ssl_cipher}) (?:-|%{NOTSPACE:ssl_protocol})" ]
  }
  date {
    match => [ "timestamp", "ISO8601" ]
  }
# don't keep duplicate data (besides request internals since these may be helpful for queries)
  mutate {
    remove_field => [ "message", "timestamp" ]
  }
# these will ensure we have a valid index even if there are upper case letters in elb names
  mutate {
    add_field => { "indexname" => "elb-%{elb}" }
  }
  mutate {
    lowercase => [ "indexname" ]
  }
}

output {
  elasticsearch {
    hosts => "ELASTICSEARCHURL"
    index   => "%{indexname}-%{+YYYY.MM.dd}" 
  }
}

```

You can see 3 main sections `input`, `filters` and `output` which support different plugins and operations. Full documentation available [here](https://www.elastic.co/guide/en/logstash/current/index.html).

Beside helping with the log processing, there is another important benefit of using logstash.
The responsability for shipping logs can be removed from the application.

Let's imagine we have an app running in 20 different servers and we want all the logs in Kibana but we don't want the application to send the logs. You can use [Filebeat](https://www.elastic.co/guide/en/beats/filebeat/5.1/filebeat-getting-started.html) or [rsyslog](http://www.rsyslog.com/) for log shipping and Logstash for processing.  


## Logstash at scale

While Logstash brings usefull capabilities it also comes with management overhead at scale. The following article, ([https://www.elastic.co/guide/en/logstash/current/deploying-and-scaling.html](https://www.elastic.co/guide/en/logstash/current/deploying-and-scaling.html)), ilustrates how a scalable logstash setup looks.


## How about Elasticsearch?

For playing around with Kibana I've used Elasticsearch as a service from AWS with a single node.
For a production environment, same as Logstash, this will need a more advanced setup with a multi node cluster. 

The master node is responsible for cluster management and limiting the writes to this mode is a good ideea.

While AWS takes care of a lot of the complexity that comes with managing ES but we will still need to manage the data. For this AWS provides automatic and manual snapshots that can be restored but this are high level backups and more related to the infrastucture than the data.

To manage cluster data and clean old indexes we can schedule jobs against the [indices api](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices.html) 


## Managed ELK services

Above I've just listed a few considerations about scale, the list is nowhere close to complete and it reveals the amount of complexity that comes with scale. Like in most cases with online tutorials the road from tutorial to operational is long.

You can use services like http://logz.io/, https://www.splunk.com/, https://www.datadoghq.com/ to deal with this complexity for you.
