Microservice Architecture Guidelines
======================

* [Basics](#basics)
* [Architecture Patterns](#architecture-patterns)
* [Best Practices](#best-practices)

## Basics

[TEXT]


### Common traits

* Focus on "one thing".
* Autonomous operation.
* Isolated development.
* Independent deployment.
* Localized decisions. (a.k.a "dumb pipes, smart endpoints")


## Architecture Patterns

Microservices offer great benefits but also raise huge new challenges. Microservice architecture patterns are fundamental pillars when creating a microservice-based application. In this section you can explore some of the most common patterns and learn about the consequences for your development team. 


### Distributed Domain-driven Design (DDDD)

[TEXT]


#### Description

* Small, self-hosted.
* Bounded contexts.
* Redundant data/CQRS.
* Business events.
* Containerized.


#### Consequences

* Loose coupling between contexts.
* Acknowledges separate evolution of contexts.
* Asynchronicity increases stability.
* Well-suited for parallel development.
* Requires a high performance Event Bus. (Kafka, RabbitMQ, etc)
* Risk creating "frontend monoliths". 


### Function as a Service (FaaS)

FaaS is the concept of serverless computing via serverless architectures. Software developers can leverage this to deploy an individual “function”, action, or piece of business logic. They are expected to start within milliseconds and process individual requests and then the process ends. Building an application following this model is typically used when building cloud-native microservices applications.


#### Description

* As small as possible.
* A few hundred lines of code or less.
* Triggered by events.
* Communication is asynchronously.
* Serverless.


#### Consequences

* Shared infrastructure dependency.
* Common interfaces, multiple invocations.
* Application logic in event handlers.
* Emerging behavior. (a.k.a "what the hell just happened")
* Billed per request.
* Unpredictable response times.


### μSOA (Nano-SOA) 

Nano-SOA fully inherits the high cohesion and low coupling characteristics of SOA architecture. Each plugin behaves like an independent service, has clear interface and boundary, and can be easily reused or replaced. It is comparable to SOA from the maintenance perspective. Each plugin can be developed and maintained separately, and a developer only needs to take care of his own plugin. By the addition of new plugins and recombination of existing plugins, nano-SOA makes things easier to modify or extend existing functions than SOA architecture. More infomation about Nano-SOA can be found [here](http://baiy.cn/doc/byasp/mSOA_en.htm).


#### Description

* Small, self-hosted.
* Communicating synchronously.
* Cascaded/streaming.
* Containerized.
* Pull-driven style requires orchestration.


#### Consequences

* Close collaboration - common goal.
* Need for resilience/stabilty patterns for invocations.
* High cost of coordination. (versioning, compatiblity, etc)
* High infrastructure demand.
* Works best in environments with extreme scalability requirements.


### SCS (Self-container Systems)

The Self-contained System (SCS) approach is an architecture that focuses on a separation of the functionality into many independent systems, making the complete logical system a collaboration of many smaller software systems. This avoids the problem of large monoliths that grow constantly and eventually become unmaintainable. Over the past few years, we have seen its benefits in many mid-sized and large-scale projects. Additional information about SCS can be found [here](http://scs-architecture.org/index.html).


#### Description

* Self-contained, autonomous.
* Includes UI + DB.
* Can be composed from smaller microservices.


#### Consequences

* Larger, independent systems, including data + UI.
* Able to autonomously serve requests.
* Light-weight integration, ideally via responsive web components.
* No extra infrastructure needed. (e.g. Kafka)
* Well suited if goal is decoupling of development teams.