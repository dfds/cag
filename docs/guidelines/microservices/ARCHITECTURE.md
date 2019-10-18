Microservice Design Guidelines
======================

* [Basics](#basics)
* [Design principles](#design-principles)
* [Design patterns](#design-patterns)

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

The design principles (non-functional requirements) are the important decision makers while designing microservices. The success of a system is completely depends on Availability, Scalability, Performance, Usability and Flexibility.


### Availability

The golden rule for availability says, anticipate failures and design accordingly so that the systems will be available for 99.999% (Five Nines). It means the system can go down only for a 5.5 minutes for an entire year. The cluster model is used to support high availability, where it suggests having group of services run in Active-Active mode or Active-Standby model.

So while designing microservices, it must be designed for appropriate clustering and high-availability model. The basic properties of microservices such as stateless, independent & full stack will help us to run multiple instances in parallel in active-active or active-standby mode.


### Scalability

Microservices must be scale-able both horizontally and vertically.    Being horizontally scale-able, we can have multiple instances of the microservice to increase the performance of the system.  The design of the microservices must support horizontal scaling (scale-out).

Also microservices should be scale-able vertically (scale-in).  If a microservice is hosted in a system with medium configuration such AWS EC2 t2-small (1-core, 2-GB memory) is moved to M4 10x-large ( 40 core & 160GB memory) it should scale accordingly.  Similarly downsizing the system capacity must also be possible.


### Performance

Performance is measured by throughput, response time (eg. 2500 TPS -transactions per second).  The performance requirements must be available in the beginning of the design phase itself. There are technologies and design choices will affect the performance. They are:

* Synchronous or Asynchronous communication (DFDS preferes the latter)
* Blocking or Non-blocking APIs (DFDS preferes the latter)
* RESTful API or RPC  (DFDS preferes the former)
* XML or JSON (DFDS preferes the latter)
* SQL or NoSQL
* Kafka or RabbitMQ  (DFDS preferes the former)
* MongoDB, Cassandra or Cosmos DB (DFDS preferes the latter)
  
So, appropriate technology and design decisions must be taken, to avoid re-work in the later stage.


### Usability

Usability aspects of the design focuses on hiding the internal design, architecture, technology and other complexities to the end user or other system.  Most of the time, microservices expose APIs to the end user as well as to other microservices.  So, the APIs must be designed in a normalized way, so that it is easy to achieve the required services with minimal number of API calls.


### Flexibility

Flexibility measures the adaptability to change.  In the microservices eco-system, where each microservice is owned by different teams and developed in agile methodology, change will happen faster than any other systems.  The microservices may not inter-operate if they don't adapt or accommodate the change in other systems.  So, there must be a proper mechanism in place to avoid such scenarios which could include publishing the APIs, documenting the functional changes, clear communication plans.


### Container design principles

In the container model, a container image instance represents a single process. By defining a container image as a process boundary, you can create primitives that can be used to scale the process or to batch it. When you design a container image, you'll see an ENTRYPOINT definition in the Dockerfile. This defines the process whose lifetime controls the lifetime of the container. When the process completes, the container lifecycle ends. Containers might represent long-running processes like web servers, but can also represent short-lived processes like batch jobs, etc.

If the process fails, the container ends, and the orchestrator takes over. If the orchestrator was configured to keep five instances running and one fails, the orchestrator will create another container instance to replace the failed process. In a batch job, the process is started with parameters. When the process completes, the work is complete. This guidance drills-down on orchestrators, later on.

You might find a scenario where you want multiple processes running in a single container. For that scenario, since there can be only one entry point per container, you could run a script within the container that launches as many programs as needed. For example, you can use Supervisor or a similar tool to take care of launching multiple processes inside a single container. However, even though you can find architectures that hold multiple processes per container, that approach isn't very common.


## Design Patterns

Microservices offer great benefits but also raise huge new challenges. Microservice architecture patterns are fundamental pillars when creating a microservice-based application. In this section you can explore some of the most common patterns and learn about the consequences for your development team. 


### Distributed Domain-driven Design (DDDD)

DDDD is essentially just a distributed variant of DDD where each individual microservice typically correlates to a specific bounded context and communication is facilitated via integration events to enable asynchronous communication between peer processes running in a k8s cluster (or similar).


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