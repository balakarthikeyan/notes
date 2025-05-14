# Virtual Machines:

A virtual machine is an emulation of a machine/physical hardware. We can run several virtual machines on a real physical machine using a hypervisor. A hypervisor is a tool that we used to create and manage virtual machines.

## Virtual Machines vs Containers:

- `Lightweight:` Docker Images are usually smaller than VM images, makes it easy to build, copy, share. `Docker Container` can start in several milliseconds, while VM starts in seconds data.
- `Layered File System:` Images have layers, and different images can share layers, make it even more space-saving and faster to build.
- `Shared OS Kernel:` All containers running on a host is indeed a bunch of processes with different file systems. They share the same OS kernel, only encapsulates system library and dependencies.

# Docker Features: 

- `Flexible:` Even the most complex applications can be containerised.
- `Lightweight:` Containers leverage and share the host kernel, making them much more efficient in terms of system resources than virtual machines.
- `Portable:` You can build locally, deploy to the cloud, and run anywhere.
- `Loosely coupled:` Containers are highly self-sufficient and encapsulated, allowing you to replace or upgrade one without disrupting others.
- `Scalable:` You can increase and automatically distribute container replicas across a datacenter.
- `Secure:` Containers apply aggressive constraints and isolations to processes without any configuration required on the part of the user.

# Docker Architecture:

Docker uses a client-server architecture. Where a client component talks with the server component using a restful API, over UNIX sockets, or a network interface. All containers on the host share the kernel of the host with respect to the operating system. 

`Example:` Linux containers run on the Linux operating system, Windows containers run on the Windows operating system, and so on.

# Fundamental Docker Concepts:

## Docker Essentials:

- `Docker` is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers. It is based on Linux containers. It uses Linux Kernel features like namespaces and control groups to create containers on top of an operating system. 

- `Docker Platform` is Docker's software that provides the ability to package and run an application in a container on any Linux server. 
    * It bundles code files and dependencies. It promotes easy scaling by enabling portability and reproducibility.

- `Docker Engine` is the client-server application. It is the layer on which Docker runs.  It runs natively on Linux system and it madeup of:
    * A `Docker Daemon` that runs in the host computer.
    * A `Docker Client` that then communicates with the `Docker Daemon` to execute commands.
    * A `REST API` for interacting with the `Docker Daemon` remotely.
    * `Docker Community Edition (CE)` is free and largely based on open source tools. 
    * `Docker Enterprise` comes with additional support, management, and security features.

- `Docker Daemon` is the Docker server that listens for Docker `REST API` requests. 
    * It manages Docker objects, such as images, containers, networks, and volumes. They can communicate using a REST API, over UNIX sockets or a network interface.
    * Actually executes commands sent to the` Docker Client`: like building, running, and distributing your containers.
    * The `Docker Client` and `Docker Daemon` can either run on the same system, or you can connect a `Docker Client` to a remote `Docker Daemon`.

- `Docker Client` that communicates with the `Docker Daemon` to execute commands.
    * When you use the `Docker Command Line Interface (CLI)` you type a command into your terminal that starts with docker. `Docker Client` then uses the Docker `REST API` to send the command to the `Docker Daemon`.

- `REST API` for interacting with the `Docker Daemon` remotely. 

- `Dockerfile` is where you write the instructions to build a `Docker Image`.

- `Docker Container` is built from a series of layers. Each layer is read only, except the final container layer that sits on top of the others. 

- `Docker Image` are read-only templates that you build from a set of instructions written in your `Dockerfile`.
    * It contains a file named `Dockerfile` with no extension.
    * It is created at build time and the `Docker Container` is created at run time.
    * Each instruction in the `Dockerfile` adds a new `layer` to the image, with layers representing a portion of the images file system that either adds to or replaces the layer below it. 
    * Layers are key to Docker's lightweight yet powerful structure. 
    * Docker uses a Union File System to achieve this.

- `Docker Volumes` are the best way to store the persistent data that your apps consume and create. 
    * It are the data part of a container, initialized when a container is created. 
    * It allow you to persist and share a container's data. Data volumes are separate from the default Union File System and exist as normal directories and files on the host filesystem. 

- `Docker Registry` is the remote location where Docker Images are stored. You push images to a registry and pull images from a registry. You can host your own registry or use a provider's registry. For example, AWS and Google Cloud have registries.

- `Docker Hub` is the largest central registry in which all the dependencies require to run the application is present anything you need you pull it from there via `Docker Daemon` and you can also push the images in `Docker Hub`
    * It's also the default registry of Docker images.

- `Docker Repository` is a collection of Docker images with the same name and different tags. The tag is the identifier for the image.

## Docker Scaling:
- `Docker Networking` allows you to connect Docker containers together. Connected Docker containers could be on the same host or multiple hosts.

- `Docker Compose` is a tool that makes it easier to run apps that require multiple Docker containers. Docker Compose allows you to move commands into a docker-compose.yml file for reuse. The Docker Compose command line interface (cli) makes it easier to interact with your multi-container app. Docker Compose comes free with your installation of Docker.

- `Docker Swarm` is a product to orchestrate container deployment. 

- `Docker Services` are the different pieces of a distributed app. Docker services allow you to scale containers across multiple Docker Daemons and make Docker Swarms possible.

# Docker Networking

- `bridge:` used to enable communication between docker containers on the same host internally.

- `host:` uses host system ports for communication.

- `none:` disables communication between docker containers.

- `overlay:` used to enable communication between docker containers on different hosts (swarm nodes).

- `macvlan:` this allows containers to grab a unique IP address from the physical LAN network with DHCP.

## Technology Used in Docker:

The programming language used in Docker is `GO`. Docker takes advantage of various features of Linux kernel like namespaces and cgroups.

`namespaces:` Docker uses namespaces to provide isolated workspace called containers. When a container is run, docker creates a set of namespaces for it, providing a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

`cgroups( Control Groups ):` cgroups are used to limit and isolate the resource usage( CPU, memory, Disk I/O, network etc ) of a collection of processes. cgroups allow Docker engine to share the available hardware resources to containers and optionally enforce limit and constraints.

`UnionFS( Union file systems ):` are file systems that operate by creating layers, making them very lightweight and fast. It is used by Docker engine to provide the building blocks for containers. Union File System as a stackable file system, meaning files and directories of separate file systems (known as branches) can be transparently overlaid to form a single file system.

`Docker Engine` combines the namespaces, cgroups, and UnionFS into a wrapper called a container format. The default container format is libcontainer.

Layered systems offer two main benefits:

`Duplication-free:` layers help avoid duplicating a complete set of files every time you use an image to create and run a new container, making instantiation of docker containers very fast and cheap.
`Layer segregation:` Making a change is much faster: when you change an image, Docker only propagates the updates to the layer that was changed.

# Install Docker on your Local Machine via Debian

## Set up a Virtual Machine:

Install a virtualization platform such as VirtualBox, VMware, or Hyper-V on your host machine. Create a new virtual machine with your preferred operating system.

## Install Docker on the Virtual Machine
```bash
# Update the Virtual Machine
sudo apt-get update
sudo apt-get upgrade

# Install required dependencies
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg software-properties-common

# Add Docker GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add Docker repository
echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker engine
sudo apt-get update

## Install the Docker packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

## To Start & Stop & Check Status
systemctl start docker.service
systemctl status docker.service
```

## How would you troubleshoot a Docker container that is stuck in an "Exited" state immediately after starting?

✅ First, check the logs of the container using `docker logs <container-id>`. The logs often provide clues about what went wrong.
✅ Check the exit code of the container using `docker inspect <container-id>` or `docker ps -a`.
✅ An `exit code of 137` typically means the container was killed due to memory limits.
✅ An `exit code of 1` often means an application error, while `exit code of 143` is typically a result of the container receiving a SIGTERM.
✅ If there are no obvious application errors, inspect the `Dockerfile` for issues with the `ENTRYPOINT` or `CMD` instructions. If these are incorrectly set or missing, the container will exit immediately.
✅ Verify if the host has enough system resources, such as CPU, memory, or disk space. You can check this with docker stats or `docker system df`.

## Observe that a container is consuming an excessive amount of memory. How do you troubleshoot this?

✅ Use the docker stats command to check the live memory usage of containers.
✅ If the memory consumption is high, add memory limits using `–memory` or `–memory-swap` flags when starting the container to restrict its memory usage.
✅ Check if there are memory leaks in the application by reviewing the application logs