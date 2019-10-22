Containerization Guidelines
======================

* [Basics](#basics)

## Basics
This document covers recommended best practices and methods for Docker and how to build images by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image.

A Docker image consists of read-only layers each of which represents a Dockerfile instruction. The layers are stacked and each one is a delta of the changes from the previous layer. Consider this Dockerfile:

´´´
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
´´´

Each instruction creates one layer:

* FROM creates a layer from the ubuntu:18.04 Docker image.
* COPY adds files from your Docker client’s current directory.
* RUN builds your application with make.
* CMD specifies what command to run within the container.
  
When you run an image and generate a container, you add a new writable layer (the “container layer”) on top of the underlying layers. All changes made to the running container, such as writing new files, modifying existing files, and deleting files, are written to this thin writable container layer.

For more on image layers (and how Docker builds and stores images), see the official Dockerfile reference [here](https://docs.docker.com/engine/reference/builder/).

### One-process-per-container
In the container model, a container image instance represents a single process. By defining a container image as a process boundary, you can create primitives that can be used to scale the process or to batch it. When you design a container image, you'll see an ENTRYPOINT definition in the Dockerfile. This defines the process whose lifetime controls the lifetime of the container. When the process completes, the container lifecycle ends. Containers might represent long-running processes like web servers, but can also represent short-lived processes like batch jobs, etc.

If the process fails, the container ends, and the orchestrator takes over. If the orchestrator was configured to keep five instances running and one fails, the orchestrator will create another container instance to replace the failed process. In a batch job, the process is started with parameters. When the process completes, the work is complete. 

Note: It is possible to create containers that launch more proccesses, but this is not encouraged behavior. If you need to keep multiple containers co-located and co-scheduled you should use an orchestrator like Kubernetes.