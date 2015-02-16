# Distributed Messaging with NSQ

## Introduction

This document aims to call out the deficiencies we've experienced with NServiceBus, how these deficiencies impact development productivity, and how they will continue to impact productivity in an environment where the majority of the work is in backend system development which depends on message queues.

## Pre-Evaluation Process

During high level feature evaluation four distributed messaging systems appeared to meet our basic needs: NSQ, Kafka, RabbitMQ, and ActiveMQ. NSQ was chosen for evaluation first because, quite frankly, it was the easiest to set up. Kafka, RabbitMQ, and ActiveMQ have not yet been evaluated.

Our basic needs are defined as:
* At-least-once guaranteed delivery
* Well documented
* Enterprise-friendly license (MIT, BSD, Apache)
* .NET client library

Nice-to-have criteria:
* Active community for support
* Cross-platform
* Open source

### High level feature evaluation notes

#### NSQ

"NSQ promotes distributed and decentralized topologies without single points of failure, enabling fault tolerance and high availability coupled with a reliable message delivery guarantee." I was able to get this up and running in a couple minutes from source and was able to produce/consume messages from the command line. A few minutes later I was doing it from code. Comes with a good set of tools for tailing the queue, pumping in messages, and a web admin interface that runs as a standalone command line tool that can run from your local box since it just uses the features of the daemon. `nsq_tail` seems interesting for real-time queue monitoring. I'm currently deep diving on this one – anything that meets the requirements and can be set up that quickly deserves some love. Developed by bit.ly the URL shortening service, ran in PROD for 2 years before they open sourced it last year. See http://nsq.io/overview/design.html and http://nsq.io/clients/building_client_libraries.html.

#### Kafka

"Apache Kafka is publish-subscribe messaging rethought as a distributed commit log." This has the potential for a lot of power. Allows replays, batch consuming, and other features I’ve barely scratched the surface on. Started at LinkedIn and later became part of Apache. See http://kafka.apache.org/documentation.html#design. Has a dependency on ZooKeeper for leader election etc. Nice slide deck about how Wikimedia uses it https://www.hakkalabs.co/tags/apache-kafka.
 
#### RabbitMQ

Concerns about RabbitMQ being weak in HA/failover have been addressed - https://www.rabbitmq.com/ha.html - although there’s still a general concern that Rabbit was built around the idea of a centralized broker (first sentence of ha.html makes that clear) and their HA solution may be retrofitted. Rabbit has a huge community though which is always a plus when you need something.

#### ActiveMQ

Supports AMQP now, they also have a .NET client which was just rev’d a few days ago. http://activemq.apache.org/nms/apachenms-api-v170.html.

#### Misc links

* http://wiki.secondlife.com/wiki/Message_Queue_Evaluation_Notes
  - A bit dated but the list of questions to ask is a good set (2010)
* http://predic8.com/activemq-hornetq-rabbitmq-apollo-qpid-comparison.htm
  - Good info on RabbitMQ and ActiveMQ (2013)
* http://www.quora.com/RabbitMQ-vs-Kafka-which-one-for-durable-messaging-with-good-query-features
  - Good info on RabbitMQ and Kafka (2012)
* http://www.bravenewgeek.com/dissecting-message-queues
  - Various MQs (2014)
