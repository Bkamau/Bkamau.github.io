---
layout: post
title:  Apache Kafka
subtitle: Easy quick intro
---
Kafka™ is used for building real-time data pipelines and streaming apps. It is horizontally scalable, fault-tolerant and wicked fast.

These are some of the terminologies that define Kafka.

![Image of Yaktocat]({{ site.baseurl }}/img/kafka.png) 

**Message** — In Kafka, messages represent the fundamental unit of data. Each message is a key/value pair. Irrespective of the data type, Kafka always converts messages into byte arrays.

**Producers** — Producers map to the publishers or the writers of the Pub/Sub architecture. They are the source that generate messages which get ingested into the system. In the context of Kafka, the producer is often referred as the client. It is important to note that the client is source of data and not to be confused with the consumer.

**Consumers** — Consumers are the subscribers or readers that receive the data. They are at the other side of the Kafka infrastructure. Kafka consumers are stateful, which means they are responsible for remembering the cursor position, which is called as an offset. The consumer is also a client of Kafka cluster. 

**Topics** — Topics represent the logical collection of messages that belong to a group. The data sent by the producers is stored in topics. Consumers subscribe to a specific topic that they are interested in.

**Partition** — Partitions are unique to Apache Kafka that are not seen the traditional message queuing systems. Each topic is split into one or more partitions. While sending data, producers don’t mention the partition but consumers are aware of the available partitions. Kafka may use the message key to automatically group similar messages into a partition. This scheme enables Kafka to dynamically scale the messaging infrastructure.

**Consumer Groups** — Consumers belong to at least one consumer group, which is typically associated with a topic. Each consumer within the group is mapped to one or more partitions of the topic. Kafka will guarantee that a message is only read by a single consumer in the group. Kafka will also ensure that all the messages belonging to the same topic are delivered to all the registered consumers of a group.

**Broker** — Each Kafka instance belonging to a cluster is called a broker. Its primary responsibility is to receive messages from producers, assigning offsets, and finally committing the messages to the disk. Based on the underlying hardware, each broker can easily handle thousands of partitions and millions of messages per second.

The partitions in a topic may be distributed across multiple brokers. This redundancy ensures the high availability of messages.

**Cluster** — A collection of Kafka broker forms the cluster. One of the brokers in the cluster is designated as a controller, which is responsible for handling the administrative operations as well as assigning the partitions to other brokers. The controller also keeps track of broker failures.
