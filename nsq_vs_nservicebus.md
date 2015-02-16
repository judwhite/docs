# Distributed Messaging with NSQ

## Introduction

This document aims to call out the deficiencies we experience with NServiceBus, how these deficiencies impact development productivity, and how they will continue to impact productivity in an environment where the majority of the work is in backend system development which depends on messaging.

The specific issues we aim to address:
* Interoperability when using the same transport (custom subscription methods, cross-team coordination, database scripts and deployment)
* Performance (enqueue and dequeue performance drops off significantly with database contention)
* Configurability (separate process needed for each queue)
* Deployment issues - startup/shutdown time
* Cryptic error messages when:
  - Configuration is wrong
  - The process crashes
  - Deployment is incomplete (missing language folders leads to Assembly scanning throwing)
* Time permitted for a message to be processed is tied to SQL Transaction timeout limit; changing machine.config is a maintenance and repeatability issue given the number of servers involved.
* Integration testing
* DTC - easy to miss in configuration, hard to spot depending on environment differences (don't use IsTransactional = false leads to lost messages on failure, no requeue)
* Issues running local with several processes
 
Excluded from the above list are issues when using MSMQ for transport, different serialization methods, or different versions of NServiceBus. These issues could be solved by standardizing on the above differences. However, these are still real issues we face today:
* MSMQ clusters are difficult to configure and deploy correctly. If a proper cluster cannot be achieved you must choose between a SPOF or message duplication; this affects all consumers of event publishers using MSMQ for transport.
* Interoperability issues when using different major versions (v3/v4/v5); for example, publishing an assembly containing message contracts requires a reference to NServiceBus to get IMessage, coupling data contracts not only with NServiceBus but specific versions.
* Interoperability when using different transports (MSMQ and SQL Server) requires a process hop.
* Serialization method is global to the process; when changing from XML to JSON or vice versa a process hop must occur to get into the target NServiceBus queue. This also affects NServiceBus configured to use SQL Server.

Recently we confirmed a bug in v4.6.1 of NServiceBus which will randomly take down a process. The bug has been patched in our version of NServiceBus, however other teams remain exposed. Upgrading to NServiceBus 5 requires anyone still on .NET 4.0 to upgrade to .NET 4.5, and the upgrade path is not light on changes - http://docs.particular.net/nservicebus/upgradeguides/4to5.



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

##### Rough notes

Simplify Configuration
Used by: bitly, docker, digg, BuzzFeed, and many others that advertise use

The real power of NSQ, that I’m sure the community can attest to, is how easy it is to build real systems on top of.  I sincerely doubt it will be your bottleneck but I can damn near guarantee you’ll enjoy working with it.

Topics - Event types "a unique stream of messages"
Channels - Event subscriptions - "a copy of that stream of messages for a given set of consumers",
  "an independent queue for a topic (a topic can have multiple channels)"
Messages - Events

"Think of a topic as a unique stream of messages or events.
Think of a channel as a copy of that stream of messages for a given set of consumers.
Topics and channels are both independent queues."

Topics and Channels are created at runtime

Transport
TCP: nsqd 4150, nsqlookupd 4160
HTTP: nsqd 4151, nsqlookupd 4161

Persistence
Disk
High water mark
Can modify to use SQL

Deployment topology/architecture
Each machine has an instance of nsqd
a single nsqd instance can have multiple topics
Each data center has a few instances of nsqlookupd

Consumers do not need their own nsqd
Consumers and Publishers can live side by side

Consumers discover producers by querying nsqlookupd (a discovery service for topics)
Consumers connect to all producers
Messages are "pushed" to Consumers
Multiple Consumers for a given channel

nsqlookupd instances are independent and require no coordination (run a few for HA)

Guarantees
- Messages are delivered at least once
- Handling is guaranteed by the protocol
  - nsqd sends a message and stores it temporarily
  - client replies FIN (finish) or REQ (re-queue)
  - if client does not reply message is automatically re-queued
- Any single nsqd instance failure can result in message loss (can be mitigated)

http://word.bitly.com/post/38385370762/spray-some-nsq-on-it
https://speakerdeck.com/snakes/nsq-nyc-golang-meetup?slide=19


Tooling

nsqadmin - web interface to administrate and introspect an NSQ cluster at runtime (empty, pause, delete channels)

nsq_tail - see what's flowing through a channel in real time
channel pausing
- stop the flow of messages from a channel to its clients
- no message loss (queue backs up)
- really $#%^ing awesome for operations

Why Queue?
(because things break)
- try to avoid SPOFs
- queues and workers are silo'd
- in failure scenarios:
  - queues provide buffering
  - workers exponentially back off
  - messages are retried
  - aka no data loss

Why would I want to invest in a different messaging technology, isn't our current one good enough even if it's not perfect? Is this going to become a distraction?

NServiceBus is a persistent distraction as we scale up the responsibilities of our backend services.
- Simpler configuration - easier to create new queues, split queues, version queues, create priority lanes, etc
- Less deployment headaches, less interoperability headaches
- Empty/pause queues at runtime through provided tooling (good for when queues build up over processing throughput or process needs fresh run); real-time queue monitoring
- Easily extensible to include workflows/sagas - multipart jobs with callbacks (we could use this in catalog denorm)
- Unit testability
- Ability to balance durability and speed depending on needs. For example, long running daily jobs could value speed, event notifications could value durability.

All real problems we face today.

The "Simple" is deceptively important since the lack of simple (or even predictable) is the cause of a lot of NSB pain. It's also a reflection of how it's exposed to the developer; simple does not imply thoughtless under the hood.

There's a lot of productivity/maintenance value here and I want to present complete answers to concerns and avoid an early dismissal.

SLOC for nsqd, nsqlookupd

http://www.davegardner.me.uk/blog/2012/11/06/stream-de-duplication/
