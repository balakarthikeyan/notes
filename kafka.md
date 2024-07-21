✅ What is Kafka?

Kafka is a distributed event store and streaming platform.
It began as an internal project at LinkedIn.
Over time, it grew rapidly and today, some of the largest data pipelines in the world use Kafka.
Organizations like Netflix and Uber rely on it for their workflows.

✅ Kafka Messages, Topics, and Partitions

The basic unit of data in Kafka is a Message
Think of a message like a record in a database table. It is transmitted as an array of bytes.
Every message goes to a particular Topic.
You can compare Kafka Topics to a database table or a folder on your computer.
Topics are also made up of multiple partitions.
Partitions improve the redundancy and make the topics horizontally scalable.

✅ Kafka Producers and Kafka Consumer

Producers in Kafka create new messages, batch them and send them over to a Kafka topic.
A producer also balances messages across the different partitions of a topic.
You can provide a custom partitioning strategy to control the distribution of messages.
Kafka Consumers read messages from a broker.
One or more consumers work as a consumer group to consume messages from a topic.
A consumer instance is tied to a particular partition. In other words, a partition is owned by a consumer instance.

✅ Kafka Broker and Cluster

A single Kafka server is known as a Broker.
A broker can handle thousands of partitions and millions of messages/second.
Think of the broker as a bridge between the producer and consumer.
It receives messages from producers and handles fetch requests from the consumer.
A Kafka Cluster consists of several brokers and provides features like replication.
Every partition is replicated across multiple brokers ensuring high-availability and redundancy.

✅ Common Use Cases of Kafka

👉 Tracking user activity on front-end application
👉 Messaging requirements in a distributed system such as notifications and emails to users
👉 Metrics collection and logging