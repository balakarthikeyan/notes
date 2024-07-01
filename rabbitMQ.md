# RabbitMQ
RabbitMQ supports a broad variety of OS systems and cloud settings, as well as a comprehensive set of developer tools for the most common programming languages.

The most extensively used open source message broker is RabbitMQ. A message broker is a computer program module that exchanges messages between message producers and consumers, allowing various software components to be effectively decoupled.

A message queue is kept for these communications. A queue is a large message buffer based on the FIFO. Messages may be sent to queues by several producers and consumers can try to acquire messages sent from these queues.

RabbitMQ is a lightweight messaging system that may be deployed on premises or in the Cloud. RabbitMQ is an open-source distributed message queue that supports many communication protocols. It is a message broker that receives messages from senders (producer) and forwards them to receivers (consumer).

# What is AMQP?  

AMQP (for Advanced Message Queuing Protocol) is an open protocol for middleware-oriented messaging systems developed by the JPMorgan Chase1 bank. The objective of AMQP is to standardize exchanges between message servers based on the following principles: oriented message, use of queues, routing (point to point and publish-subscribe), reliability, and security.

# Docker Installation
```
docker pull rabbitmq:3-management
docker run --rm -it --hostname my-rabbit -p 15672:15672 -p 5672:5672 rabbitmq:3-management

To Run:
http://localhost:15672

Default username and password as credentials:
guest:guest 

```

The port 5672 is used to make connections with clients for RabbitMQ, while the port 15672 is used for the management website of RabbitMQ

# Type of queue:

`Cyclic queue` has a wheel structure, if it comes down to the last element of the list it automatically starts from the beginning.
`Double-sided queue` both sides can be used for adding as well as deleting nodes.
`Priority queue` elements are added according to a given pre-defined priority which means that element with a higher priority will be moved up higher in a hierarchy and will be serviced earlier, not like in a classic queue when it is added up to the end of a queue.

# Symfony Messenger Concepts:

`Message` – any object
`Bus` – dispatching messages
`Message Handler` – processes for servicing messages
`Transport` – sending and receiving messages, sending to queues for example via RabbitMQ
`Worker` – processing and consuming messages

# Symfony Installation
```
composer require symfony/amqp-messenger
composer req messenger amqp
php bin/console debug:messenger
php bin/console messenger:consume -v
php bin/console messenger:consume async -vv
php bin/console messenger:consume external_messages 
```

the message class is the class that holds the message’s data;
the handler class is in charge of reading the Message. It also will delegate the action to one or more services;
the serializer class is where the information gets translated into a readable version for our application to use it;
transport is what we need for sending and receiving messages;