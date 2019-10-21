DFDS Microservice Taxonomy
======================

This section contains information about the on-going development and maintenance of DFDS Composable Architecture taxonomy for application developers.

![alt text](https://github.com/dfds/cag/blob/master/docs/images/Composable_Architecture_Guidelines_Taxonomy.svg "Taxonomy - CAG")

| Term       | Meaning  |
| :------------- | :----------: |
|  Capability    | <div align="left">A capability is a logical grouping of microservices that collaborate in order to sustain a given business process.</div>  |
|  Namespace     | <div align="left">A namespace is a virtual cluster that is assigned directly to a given capability. Providing a way to clearly divide computing resources between that various capabilities running in the DFDS organization.</div> |
|  Services      | <div align="left">Services are a virtual manifestation of the public endpoints exposed by pods belonging to a given capability inside its associated namespace.</div> |
|  Pods          | <div align="left">A Pod is a group of one or more containers (such as Docker containers), with shared storage/network, and a specification for how to run the containers. A Pod’s contents are always co-located (same node) and co-scheduled (same time), and run in a shared context.</div> |
|  Containers    | <div align="left">A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.</div> |
|  Contexts      | <div align="left">A context is a logical grouping of software that belong to a given capability. Also know as Bounded Contexts they provide a boundary for bundling domain services that don’t have any conflicting model, while allowing an internal Ubiquitous Languages to evolve freely without creating ambiguity in the capability as a whole.</div> |
|  Events        | <div align="left">Events keep state in sync across multiple contexts or external systems. This is done by publishing integration events from a given context for others to consume. Often the contents of these integration events are simply an aggregation of multiple domain events.</div> |
|  Code          | <div align="left">Code is a "generic term" for the assemblies that contain the software used by one or more contexts.</div> |