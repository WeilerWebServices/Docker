![Docker Logo](Docker.png)

# Docker

---

Docker overview

---

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

---

# The Docker platform[](https://docs.docker.com/get-started/overview/#the-docker-platform)

Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight because they don't need the extra load of a hypervisor, but run directly within the host machine's kernel. This means you can run more containers on a given hardware combination than if you were using virtual machines. You can even run Docker containers within host machines that are actually virtual machines!

Docker provides tooling and a platform to manage the lifecycle of your containers:

-   Develop your application and its supporting components using containers.
-   The container becomes the unit for distributing and testing your application.
-   When you're ready, deploy your application into your production environment, as a container or an orchestrated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.

---

# Docker Engine[](https://docs.docker.com/get-started/overview/#docker-engine)

*Docker Engine* is a client-server application with these major components:

-   A server which is a type of long-running program called a daemon process (the `dockerd` command).

-   A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

-   A command line interface (CLI) client (the `docker` command).

![Docker Engine Components Flow](https://docs.docker.com/engine/images/engine-components-flow.png)

The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI.

The daemon creates and manages Docker *objects*, such as images, containers, networks, and volumes.

> Note: Docker is licensed under the open source Apache 2.0 license.

For more details, see [Docker Architecture](https://docs.docker.com/get-started/overview/#docker-architecture) below.

---

# What can I use Docker for?[](https://docs.docker.com/get-started/overview/#what-can-i-use-docker-for)

Fast, consistent delivery of your applications

Docker streamlines the development lifecycle by allowing developers to work in standardized environments using local containers which provide your applications and services. Containers are great for continuous integration and continuous delivery (CI/CD) workflows.

Consider the following example scenario:

-   Your developers write code locally and share their work with their colleagues using Docker containers.
-   They use Docker to push their applications into a test environment and execute automated and manual tests.
-   When developers find bugs, they can fix them in the development environment and redeploy them to the test environment for testing and validation.
-   When testing is complete, getting the fix to the customer is as simple as pushing the updated image to the production environment.

Responsive deployment and scaling

Docker's container-based platform allows for highly portable workloads. Docker containers can run on a developer's local laptop, on physical or virtual machines in a data center, on cloud providers, or in a mixture of environments.

Docker's portability and lightweight nature also make it easy to dynamically manage workloads, scaling up or tearing down applications and services as business needs dictate, in near real time.

Running more workloads on the same hardware

Docker is lightweight and fast. It provides a viable, cost-effective alternative to hypervisor-based virtual machines, so you can use more of your compute capacity to achieve your business goals. Docker is perfect for high density environments and for small and medium deployments where you need to do more with fewer resources.

---

# Docker architecture[](https://docs.docker.com/get-started/overview/#docker-architecture)

Docker uses a client-server architecture. The Docker *client* talks to the Docker *daemon*, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon *can* run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.

![Docker Architecture Diagram](https://docs.docker.com/engine/images/architecture.svg)

### The Docker daemon[](https://docs.docker.com/get-started/overview/#the-docker-daemon)

The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

### The Docker client[](https://docs.docker.com/get-started/overview/#the-docker-client)

The Docker client (`docker`) is the primary way that many Docker users interact with Docker. When you use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out. The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

### Docker registries[](https://docs.docker.com/get-started/overview/#docker-registries)

A Docker *registry* stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the `docker pull` or `docker run` commands, the required images are pulled from your configured registry. When you use the `docker push` command, your image is pushed to your configured registry.

### Docker objects[](https://docs.docker.com/get-started/overview/#docker-objects)

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.

#### IMAGES

An *image* is a read-only template with instructions for creating a Docker container. Often, an image is *based on* another image, with some additional customization. For example, you may build an image which is based on the `ubuntu` image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a *Dockerfile* with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

#### CONTAINERS

A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container's network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

##### Example `docker run` command

The following command runs an `ubuntu` container, attaches interactively to your local command-line session, and runs `/bin/bash`.

```
$ docker run -i -t ubuntu /bin/bash

```

When you run this command, the following happens (assuming you are using the default registry configuration):

1.  If you do not have the `ubuntu` image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.

2.  Docker creates a new container, as though you had run a `docker container create` command manually.

3.  Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem.

4.  Docker creates a network interface to connect the container to the default network, since you did not specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine's network connection.

5.  Docker starts the container and executes `/bin/bash`. Because the container is running interactively and attached to your terminal (due to the `-i` and `-t` flags), you can provide input using your keyboard while the output is logged to your terminal.

6.  When you type `exit` to terminate the `/bin/bash` command, the container stops but is not removed. You can start it again or remove it.

#### SERVICES

Services allow you to scale containers across multiple Docker daemons, which all work together as a *swarm* with multiple *managers* and *workers*. Each member of a swarm is a Docker daemon, and all the daemons communicate using the Docker API. A service allows you to define the desired state, such as the number of replicas of the service that must be available at any given time. By default, the service is load-balanced across all worker nodes. To the consumer, the Docker service appears to be a single application. Docker Engine supports swarm mode in Docker 1.12 and higher.

---

# The underlying technology[](https://docs.docker.com/get-started/overview/#the-underlying-technology)

Docker is written in the [Go programming language](https://golang.org/) and takes advantage of several features of the Linux kernel to deliver its functionality.

### Namespaces[](https://docs.docker.com/get-started/overview/#namespaces)

Docker uses a technology called `namespaces` to provide the isolated workspace called the *container*. When you run a container, Docker creates a set of *namespaces* for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

Docker Engine uses namespaces such as the following on Linux:

-   The `pid` namespace: Process isolation (PID: Process ID).
-   The `net` namespace: Managing network interfaces (NET: Networking).
-   The `ipc` namespace: Managing access to IPC resources (IPC: InterProcess Communication).
-   The `mnt` namespace: Managing filesystem mount points (MNT: Mount).
-   The `uts` namespace: Isolating kernel and version identifiers. (UTS: Unix Timesharing System).

### Control groups[](https://docs.docker.com/get-started/overview/#control-groups)

Docker Engine on Linux also relies on another technology called *control groups* (`cgroups`). A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.

### Union file systems[](https://docs.docker.com/get-started/overview/#union-file-systems)

Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.

### Container format[](https://docs.docker.com/get-started/overview/#container-format)

Docker Engine combines the namespaces, control groups, and UnionFS into a wrapper called a container format. The default container format is `libcontainer`. In the future, Docker may support other container formats by integrating with technologies such as BSD Jails or Solaris Zones.

---

# Get Docker

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

You can download and install Docker on multiple platforms. Refer to the following section and choose the best installation path for you.

[![Docker Desktop for Mac](https://docs.docker.com/images/apple_48.svg)](https://docs.docker.com/docker-for-mac/install/)

[Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/install/)[](https://docs.docker.com/get-docker/#docker-for-mac)

---

# A native application using the macOS sandbox security model which delivers all Docker tools to your Mac.

[![Docker Desktop for Windows](https://docs.docker.com/images/windows_48.svg)](https://docs.docker.com/docker-for-windows/install/)

[Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/)[](https://docs.docker.com/get-docker/#docker-for-windows/install/)

---

# A native Windows application which delivers all Docker tools to your Windows computer.

[![Docker for Linux](https://docs.docker.com/images/linux_48.svg)](https://docs.docker.com/engine/install/)

[Docker for Linux](https://docs.docker.com/engine/install/)[](https://docs.docker.com/get-docker/#docker-for-linux)

---

# Install Docker on a computer which already has a Linux distribution installed.

[Docker](https://docs.docker.com/search/?q=Docker), [download](https://docs.docker.com/search/?q=download), [documentation](https://docs.docker.com/search/?q=documentation), [manual](https://docs.docker.com/search/?q=manual)

---

# Orientation and setup

This page contains step-by-step instructions on how to get started with Docker. We also recommend the video walkthrough from DockerCon 2020.

In this tutorial, you'll learn about creating and deploying Docker apps, including using multiple containers with a database, and using Docker Compose. You'll also deploy your containerized app to Azure.

---

# Start the tutorial[](https://docs.docker.com/get-started/#start-the-tutorial)

If you've already run the command to get started with the tutorial, congratulations! If not, open a command prompt or bash window, and run the command:

```
docker run -d -p 80:80 docker/getting-started

```

You'll notice a few flags being used. Here's some more info on them:

-   `-d` - run the container in detached mode (in the background)
-   `-p 80:80` - map port 80 of the host to port 80 in the container
-   `docker/getting-started` - the image to use

> Pro tip
>
> You can combine single character flags to shorten the full command. As an example, the command above could be written as:
>
> ```
> docker run -dp 80:80 docker/getting-started
>
> ```

---

# The Docker Dashboard[](https://docs.docker.com/get-started/#the-docker-dashboard)

Before going too far, we want to highlight the Docker Dashboard, which gives you a quick view of the containers running on your machine. It gives you quick access to container logs, lets you get a shell inside the container, and lets you easily manage container lifecycle (stop, remove, etc.).

To access the dashboard, follow the instructions for either [Mac](https://docs.docker.com/docker-for-mac/dashboard/) or [Windows](https://docs.docker.com/docker-for-windows/dashboard/). If you open the dashboard now, you will see this tutorial running! The container name (`jolly_bouman` below) is a randomly created name. So, you'll most likely have a different name.

![Tutorial container running in Docker Dashboard](https://docs.docker.com/get-started/images/tutorial-in-dashboard.png)

---

# What is a container?[](https://docs.docker.com/get-started/#what-is-a-container)

Now that you've run a container, what *is* a container? Simply put, a container is simply another process on your machine that has been isolated from all other processes on the host machine. That isolation leverages [kernel namespaces and cgroups](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504), features that have been in Linux for a long time. Docker has worked to make these capabilities approachable and easy to use.

!!! info "Creating Containers from Scratch" If you'd like to see how containers are built from scratch, Liz Rice from Aqua Security has a fantastic talk in which she creates a container from scratch in Go. While she makes a simple container, this talk doesn't go into networking, using images for the filesystem, and more. But, it gives a *fantastic* deep dive into how things are working.

```
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/8fi7uSYlOdc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

```

---

# What is a container image?[](https://docs.docker.com/get-started/#what-is-a-container-image)

When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container's filesystem, it must contain everything needed to run an application - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.

We'll dive deeper into images later on, covering topics such as layering, best practices, and more.

!!! info If you're familiar with `chroot`, think of a container as an extended version of `chroot`. The filesystem is simply coming from the image. But, a container adds additional isolation not available when simply using chroot.

---

# CLI references[](https://docs.docker.com/get-started/#cli-references)

Refer to the following topics for further documentation on all CLI commands used in this article:

-   [docker version](https://docs.docker.com/engine/reference/commandline/version/)
-   [docker run](https://docs.docker.com/engine/reference/commandline/run/)
-   [docker image](https://docs.docker.com/engine/reference/commandline/image/)
-   [docker container](https://docs.docker.com/engine/reference/commandline/container/)

[get started](https://docs.docker.com/search/?q=get%20started), [setup](https://docs.docker.com/search/?q=setup), [orientation](https://docs.docker.com/search/?q=orientation), [quickstart](https://docs.docker.com/search/?q=quickstart), [intro](https://docs.docker.com/search/?q=intro), [concepts](https://docs.docker.com/search/?q=concepts), [containers](https://docs.docker.com/search/?q=containers), [docker desktop](https://docs.docker.com/search/?q=docker%20desktop)

---
