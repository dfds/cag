Design Principles
======================

* [Basics](#basics)
* [Design principles](#design-principles)

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
The design principles (non-functional requirements) are the important decision makers while designing microservices. The success of a system is highly dependant on adhering to these principles.

### Event-driven
An event-driven architecture is a publisher/consumer paragdim that can be used to communicate state changes between distributed processes. The publisher, which is the source of the event, only knows that the event has occurred. Consumers are entities that need to know the event has occurred; they may be involved in processing the event or they may simply be affected by the event. 

Event consumers typically subscribe to some type of middleware event manager, e.g. a topic. When the manager receives notification of an event from a publisher, it forwards that event to all registered consumers. The benefit of an event-driven architecture is that it enables large numbers of creators and publishers to exchange status and response information in near real-time.

### Cloud-native
In general usage, “cloud-native” is an approach to building and running applications that exploits the advantages of the cloud computing delivery model by packaging all its dependencies in its own container, which is dynamically orchestrated so each part is actively scheduled and managed to optimize resource utilization, and microservices-oriented to increase the overall agility and maintainability of applications. By taking advantage of cloud services in this way we enable scalable components to deliver discrete and reusable features that integrate in well-described ways, even across technology boundaries like multicloud.

### Availability
The golden rule for availability says, anticipate failures and design accordingly so that the systems will be available for 99.999% (Five Nines). It means the system can go down only for a 5.5 minutes for an entire year. The cluster model is used to support high availability, where it suggests having group of services run in Active-Active mode or Active-Standby model.

So while designing microservices, it must be designed for appropriate clustering and high-availability model. The basic properties of microservices such as stateless, independent & full stack will help us to run multiple instances in parallel in active-active or active-standby mode.

### Scalability
Microservices must be scale-able both horizontally and vertically. Being horizontally scale-able, we can have multiple instances of the microservice to increase the performance of the system.  The design of the microservices must support horizontal scaling (scale-out).

Also microservices should be scale-able vertically (scale-in). If a microservice is hosted in a system with medium configuration such AWS EC2 t2-small (1-core, 2-GB memory) is moved to M4 10x-large ( 40 core & 160GB memory) it should scale accordingly.  Similarly downsizing the system capacity must also be possible.

### Performance
Performance is measured by throughput, response time (eg. 2500 TPS -transactions per second). The performance requirements must be available in the beginning of the design phase itself. There are technologies and design choices will affect the performance. They are:

* Synchronous or Asynchronous communication
* Blocking or Non-blocking APIs
* RESTful API or gRPC
* XML or JSON
* SQL or NoSQL
* Kafka or RabbitMQ
* MongoDB, Cassandra or Cosmos DB
  
So, appropriate technology and design decisions must be taken, to avoid re-work in the later stage.

### Usability
Usability aspects of the design focuses on hiding the internal design, architecture, technology and other complexities to the end user or other system.  Most of the time, microservices expose APIs to the end user as well as to other microservices.  So, the APIs must be designed in a normalized way, so that it is easy to achieve the required services with minimal number of API calls.

### Flexibility
Flexibility measures the adaptability to change.  In the microservices eco-system, where each microservice is owned by different teams and developed in agile methodology, change will happen faster than any other systems.  The microservices may not inter-operate if they don't adapt or accommodate the change in other systems.  So, there must be a proper mechanism in place to avoid such scenarios which could include publishing the APIs, documenting the functional changes, clear communication plans.