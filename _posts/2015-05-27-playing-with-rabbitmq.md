---
layout: post
title: Hands on with RabbitMQ
description: ""
category: 
  - messaging
tags: "notes,messaging"
published: true
headline: ""
modified: ""
categories: 
  - "null"
imagefeature: ""
mathjax: false
featured: true
comments: true
---





# Setup

The [Management plugin](https://www.rabbitmq.com/management.html) provides a web interface available at http://localhost:15672/#/

**Command** - start server 

{% highlight bash %}
C:\Program Files (x86)\RabbitMQ Server\rabbitmq_server-3.5.1\sbin>rabbitmq-server.bat
{% endhighlight %}

### Work/Task Queue

- Used to distribute tasks between multiple workers.
- Round robin dispatching
- Message acknowledgement
- Message durability
- Fair dispatch


### Exchanges

A producer never send messages to queues in RabbitMQ, it sends them to an Exchange and the exchange sends the messages to a queue (bindings).

Exchange types
 - direct
 - topic
 - headers
 - fanout

Command: `rabbitmqctl list_exchanges`

#### Direct exchanges

Message goes to the queue whose binding key exactly matches the routing key of the message.

#### Topic Exchange

Direct exchanges and bindings does not allow us to do routing based on multiple criteria.

### Bindings

The binding is the relationship between the exchange and the queue.

**Command** - define - `channel.QueueBind(queueName, "logs", "");`

**Command**  - list - `rabbitmqctl list_bindings`

**Command** - Bindings can take an extra routingKey parameter -  `channel.QueueBind(queueName, "direct_logs", "black");`

# How a demo might look like

 - producer: web api which  pushes messages to RabbitMQ
 - consumer0: validates the message and sends the message in the processing pipeline
 - consumer1: c1 logic and based on the result moves the message to c2 or success queue
 - consumer2: c2 logic and moves message to success queue or fail queue
 - use a [Smart Proxy](http://www.enterpriseintegrationpatterns.com/SmartProxy.html) or a [Control Bus](http://www.enterpriseintegrationpatterns.com/ControlBus.html) for system monitoring

# Sample base consumer

Initializes connection to Rabbit MQ, receives message, fires of event for message processing and publishes another message.

{% highlight csharp %}
public class Consumer
{
  private static readonly Logger _logger = LogManager.GetCurrentClassLogger();
  public event EventHandler<CanonicalModel> OnReceive;
  private IModel _exchange;

  public void Init(string incoming = "#")
  {
    var factory = new ConnectionFactory() { HostName = "localhost" };
    using (var connection = factory.CreateConnection())
      using (_exchange = connection.CreateModel())
      {
        _exchange.ExchangeDeclare("exchange_name", "topic");
        var queueName = _exchange.QueueDeclare().QueueName;
        _exchange.QueueBind(queueName, "queue_name", incoming);

        var consumer = new QueueingBasicConsumer(_exchange);
        _exchange.BasicConsume(queueName, true, consumer);

        while (true)
        {
          var ea = consumer.Queue.Dequeue();
          var model = new CanonicalModel(ea.Body);

          _logger.Info(new { Time = DateTime.Now, Type = "IN", Value = "..." });

          var startTime = DateTime.Now;
          if (OnReceive != null)
          {
            OnReceive(this, model);
          }

          _logger.Info(new { Time = DateTime.Now, Type = "CONTROL", Value = DateTime.Now.Subtract(startTime).Milliseconds });
        }
      }
  }

  public void Publish(string routingKey, CanonicalModel model)
  {
    var json = JsonConvert.SerializeObject(model);
    var message = Encoding.UTF8.GetBytes(json);

    _logger.Info(new { Time = DateTime.Now, Type = "OUT", Value = "..." });
    _exchange.BasicPublish("exchange_name", routingKey, null, message);
  }
}
{% endhighlight %}

# Processing consumer


{% highlight csharp %}
class ConsoleConsumer
{
  private const string SERVICENAME = "CONSUMER_FOO";
  private const string INCOMING = "foo";
  private const string OK = "success";
  private const string FAIL = "fail";
  private static Consumer _consumer;

  static void Main(string[] args)
  {

    Console.WriteLine(" [Running] {0} ...", SERVICENAME);

    _consumer = new Consumer();
    _consumer.OnReceive += Process;
    _consumer.Init(INCOMING);

  }

  public static void Process(object sender, CanonicalModel model)
  {
    var result = true;
    var outRoutingKey = result ? OK : FAIL;

    _consumer.Publish(outRoutingKey, model);
  }
}
{% endhighlight %}
