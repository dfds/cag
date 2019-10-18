Microservice Design Guidelines
======================

* [Basics](#basics)
* [Architecture Patterns](#architecture-patterns)
* [Best Practices](#best-practices)

## Basics

As the name implies, a microservices architecture is an approach to building a server application as a set of small services. That means a microservices architecture is mainly oriented to the back-end, although the approach is also being used for the front end. Each service runs in its own process and communicates with other processes using protocols such as HTTP/HTTPS, WebSockets, or AMQP. Each microservice implements a specific end-to-end domain or business capability within a certain context boundary, and each must be developed autonomously and be deployable independently. Finally, each microservice should own its related domain data model and domain logic (sovereignty and decentralized data management) and could be based on different data storage technologies (SQL, NoSQL) and different programming languages.

What size should a microservice be? When developing a microservice, size shouldn't be the important point. Instead, the important point should be to create loosely coupled services so you have autonomy of development, deployment, and scale, for each service. Of course, when identifying and designing microservices, you should try to make them as small as possible as long as you don't have too many direct dependencies with other microservices. More important than the size of the microservice is the internal cohesion it must have and its independence from other services.

Why a microservices architecture? In short, it provides long-term agility. Microservices enable better maintainability in complex, large, and highly-scalable systems by letting you create applications based on many independently deployable services that each have granular and autonomous lifecycles.

As an additional benefit, microservices can scale out independently. Instead of having a single monolithic application that you must scale out as a unit, you can instead scale out specific microservices. That way, you can scale just the functional area that needs more processing power or network bandwidth to support demand, rather than scaling out other areas of the application that don't need to be scaled. That means cost savings because you need less hardware.

![alt text](https://github.com/dfds/cag/blob/master/docs/images/monolith-deployment-vs-microservice-approach.png)


### Common traits

* Focus on "one thing".
* Autonomous operation.
* Isolated development.
* Independent deployment.
* Localized decisions. (a.k.a "dumb pipes, smart endpoints")

## Design principles

[TEXT]


### Container design principles

In the container model, a container image instance represents a single process. By defining a container image as a process boundary, you can create primitives that can be used to scale the process or to batch it. When you design a container image, you'll see an ENTRYPOINT definition in the Dockerfile. This defines the process whose lifetime controls the lifetime of the container. When the process completes, the container lifecycle ends. Containers might represent long-running processes like web servers, but can also represent short-lived processes like batch jobs, etc.

If the process fails, the container ends, and the orchestrator takes over. If the orchestrator was configured to keep five instances running and one fails, the orchestrator will create another container instance to replace the failed process. In a batch job, the process is started with parameters. When the process completes, the work is complete. This guidance drills-down on orchestrators, later on.

You might find a scenario where you want multiple processes running in a single container. For that scenario, since there can be only one entry point per container, you could run a script within the container that launches as many programs as needed. For example, you can use Supervisor or a similar tool to take care of launching multiple processes inside a single container. However, even though you can find architectures that hold multiple processes per container, that approach isn't very common.


## Design Patterns

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