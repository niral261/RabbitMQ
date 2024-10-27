# RabbitMQ
RabbitMQ follows Publish-Subscribe Pattern for Communication, so before jumping into RabbitMQ, let’s understand what this publish-subscribe mechanism truly is,

## Publish-Subscribe Model
In distributed applications, components of the system often need to provide information to other components as events happen. For this, message passing comes into the picture, it is an effective way to decouple senders from consumers and avoid blocking the sender to wait for a response.

However, it leads to some questions,

👉 Scalability: Using a dedicated message queue for each consumer does not conclusively scale to many consumers.

👉 Abstraction: some of the consumers might be interested in only a subset of the information available in the queue.

👉 Coupling: Sometimes, senders want to broadcast events to all interested parties without knowing their locations/addresses. If the sender stores the location mapping, then it will lead to a tightly coupled system.

We can solve these problems using an asynchronous messaging mechanism that includes the following:

An input messaging channel is used by the sender. The sender compiles the event-related data into messages, using a pre-defined message format and sends these messages via the transmission channel. The senders in this mechanism are also called the publishers.

On the other hand, these messages can be received by consumers. The consumers are known as subscribers.

A communication medium for copying each message from the input channel to the output channels for all subscribers interested in that message is known as a message broker or event bus.

The following diagram shows the logical components of this mechanism:
![image](https://github.com/user-attachments/assets/9c2dd283-fb18-4726-9023-934d9223959e)

## Advantages of Pub-Sub:

Decoupled/loosely cooped components: Through the pub-sub mechanism, the subsystem gets decoupled and it can be easily managed through independent and asynchronous communication It decouples subsystems that still need to communicate.
Scalable and highly responsible: The sender (publisher) of the message can quickly send the message to the communication channel and immediately return to its core processing responsibilities. It does not have to worry about the delivery of the messages. The messaging infrastructure will have the responsibility to deliver messages to interested parties.
Routing, deferred/scheduled processing: Subscribers can wait to pick up messages until off-peak hours, or messages can be routed or processed according to a specific schedule.
Fault tolerance: It enables systems to perform smoothly under increased loads and handle intermittent failures more efficiently, making the system more reliable and fault-tolerant.
Workflow compatibility: It provides asynchronous workflow orchestration across distributed applications.
Separation of concerns: Each application can focus on its core capabilities, while the messaging infrastructure handles everything required to reliably route messages to multiple consumers.
This publisher-subscriber mechanism is widely used within n/w operations, automation, data centers, and other industries that need real-time computing. It is also a very useful pattern for serverless computing and distributed cloud-based systems. RabbutMQ is one of the technologies which internally implements this pattern, Let’s look into rabbitmq in more detail.

## RabbitMQ
RabbitMQ is a message broker software that uses AMQP protocol to transfer data. i.e, RabbitMQ accepts messages from producers and delivers them to consumers. It is widely used because it is open source and free of cost unlike its competitors — Amazon MQ or GC Pub/Sub.

RabbitMQ acts as a middleman which is used to reduce delivery times and loads taken by application servers. It gives your applications a common interface to send and receive messages, and your messages a safe place (i.e. a queue) to live until received.

## About AMQP

AMQP is Advanced Message Queuing Protocol which is a messaging protocol that enables client applications to communicate with messaging middleware brokers.

Messaging brokers receive messages from publishers and route them to consumers.

AMQP has Queues, exchanges, and bindings which are collectively referred to as its entities. These entities help us to translate messages from producer to consumer.

## RabbitMQ Components
RabbitMQ comes with the following 4 basic components:

– Producer

– Exchange

– Queue

– Consumer

![image](https://github.com/user-attachments/assets/2fe2e590-6405-4df4-bb35-9e7cdd63f9f5)

👉 Producer

A producer is the user application that publishes the messages. We send our message from the publisher. The publisher will connect to RabbitMQ, send a single message, then exit.

When publishing a message, publishers may specify various message attributes (message meta-data). Some of these attributes can be consumed by RabbitMQ (broker) but many will be only consumed by the Receiver.

👉 Exchange

The broker from the pub-sub mechanism is known as Exchange. It also has a Queue to store messages.

Exchanges take a message from the producer and route it into zero or more queues. The routing algorithm used depends on the exchange type and rules called bindings. This algorithm also has a routing key associated with it. It is nothing but a message attribute added to the message header by the producer. We can visualize the routing key as an “address” that the exchange is using to decide how to route the message. A message goes to the queue(s) with the binding key that exactly matches the routing key of the message.

There are the following types of exchange types:

– Direct exchange: When we have to send messages to queue only when specific key’s(routing key) value matches between them.

– Fanout exchange: When we want to broadcast our message So that any queue can receive the message.

– Topic exchange: Unlike Direct exchange, they match pattern instead of an exact key.

– Headers exchange: The value of the header equals the value specified upon the routing key.


👉 Queue

A queue is message storage/buffer for RabbitMQ. When messages flow through RabbitMQ components, they can only be stored inside a queue.

Many producers can send messages that go to one queue, and many consumers can try to receive data from one queue. It is essentially only bound by the host’s memory & disk limits.

Before a queue can be used it has to be declared. Declaring a queue will cause it to be created if it does not already exist. The re-declaration will have no effect if the queue does already exist and its attributes are the same as those in the declaration.

👉 Receiver

A consumer is a program that mostly waits to receive messages (i.e. subscribed to the messages). Consumer listens for messages from RabbitMQ queues based on different policies attached. Whenever there are new messages in the queue that the receiver is listening to, it will be able to pull those messages from the queue in the sequence that they were inserted into that queue.

Also, when a new consumer is added, assuming there are already messages ready in the queue, deliveries will start immediately.
