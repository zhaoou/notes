# AMQP and RabbitMQ

#### Advanced Message Queueing Protocol is a language agnostic messaging standard.

#### RabbitMQ is a popular open source implementation of AMQP.

In AMQP, producers publish message to an exchange, that can be bound to one or many queues, consumers pull messages from the queue.


    Producer ---> Exchange ----> Queue ----> Consumer
                          __
                            \
                             Queue is bound to Exchange using Binding Key
                              

When a message is sent to exchange, it has `routing key`.

##### Routing key is evaluated agains Binding keys, to know where to route this message, by exchange.

There are 4 main exchange types: *Direct, Topic, Headers, and Fanout*.

* Direct: message routing key must equal binding key

* Topic: messages routing key is a wildcard match of binding key

* Headers: headers in message must match headers in binding. `x-match` specifies if all or some headers must match.
 
* Fanout: message goes to all bound queues.

To get started using RabbitMQ in Spring, we need to configure connection factory, and give host, port, username and password.
We can then create and configure queues, exchanges, and bindings in our configuration, if needed.

The simplest configuration is creating one queue. 

*By default, it is bound to the default exchange, with the routing key equal to queue's name.*

### Sending messages

1) Declare a RabbitTemplate, giving it connection factory

2) Autowire where needed

3) Use `convertAndSend`:

```
// optional arguments: exchange.name and routing.key can be configured to use defaults.
rabbit.convertAndSend("exchange.name", "routing.key", message); 
```
OR `send` that takes an instance of `org.springframework.amqp.core.Message`

```
Message helloMessage = new Message("Hello World!".getBytes(), new MessageProperties());
rabbit.send("hello.exchange", "hello.routing", helloMessage);
```

### Receiving messages

#### Synchronous:

On the other side, we can use rabbit template as well:

```
Message message = rabbit.receive("queue.name");
```
This gives us `org.springframework.amqp.core.Message`.

Or we can get objects that we send: 
```
Spittle spittle = (Spittle) rabbit.receiveAndConvert("spittle.alert.queue");
```

#### Asynchronous:

We can define message driven POJO listening to one or more queues.

1) Define connection factory

2) Define listener POJO with `onMessage(org.springframework.amqp.core.Message message)` method

3) Define ListenerContainer and give it references to connectionFactory and listener



