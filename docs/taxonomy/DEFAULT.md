This section contains information about the on-going development and maintenance of DFDS Composable Architecture taxonomy for application developers.

![alt text](https://github.com/dfds/cag/blob/master/docs/images/Composable_Architecture_Guidelines_Taxonomy.svg "Taxonomy - CAG")

| Term       | Meaning  |
| :------------- | :----------: |
|  Capability    | A capability is a logical grouping of microservices that collaborate in order to sustain a given business process.   |
|  Namespace     | A namespace is a virtual cluster that is assigned directly to a given capability. Providing a way to clearly divide computing resources between that various capabilities running in the DFDS organization. |
|  Services      | Services are a virtual manifestation of the public endpoints exposed by pods belonging to a given capability inside its associated namespace. |
|  Pods          | A Pod is a group of one or more containers (such as Docker containers), with shared storage/network, and a specification for how to run the containers. A Pod’s contents are always co-located and co-scheduled, and run in a shared context. |
|  Containers    | A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. |
|  Contexts      | A context is a logical grouping of software that belong to a given capability. Also know as Bounded Contexts they provide a boundary for bundling domain services that don’t have any conflicting model, while allowing an internal Ubiquitous Languages to evolve freely without creating ambiguity in the capability as a whole. |
|  Events        | Events keep state in sync across multiple contexts or external systems. This is done by publishing integration events from a given context for others to consume. Often the contents of these integration events are simply an aggregation of multiple domain events. |
|  Code          | Code is a "generic term" for the collection of assemblies that controls the internals of a given context. |