# What is Docker?

Docker is a set of platform as a service product that use OS-level virtualisation to deliver software in packages called containers. 
By using Docker, developers can build, pack, share, deploy and run applications as lightweight, portable, self-sufficient containers and running virtually anywhere.

# What is a Docker Image?
In Docker, An image is a combination of a file system and parameters. It is a single file with all the dependencies and configurations required to run a program.

# What is a Container?
A container is an instance of an image, which is a running program. 

## Containers:

Containers allow developers to package an application with its dependencies and deploy it as a single unit. Docker is used to automate the deployment of software in lightweight containers. The use of containers to deploy applications is called containerisation. It packages applications as images that contain code, runtime environment, libraries, and configuration.

Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. All containers are run by a single operating-system kernel and are thus more lightweight than virtual machines.

## Virtual Machines:

A virtual machine is an emulation of a machine/physical hardware. We can run several virtual machines on a real physical machine using a hypervisor. A hypervisor is a tool that we used to create and manage virtual machines.

## Virtual Machines vs Containers:

Lightweight: Docker images are usually smaller than VM images, makes it easy to build, copy, share. Docker containers can start in several milliseconds, while VM starts in seconds data.

Layered File System: Images have layers, and different images can share layers, make it even more space-saving and faster to build. If all containers use Ubuntu as their base images, not every image has its own file system, but share the same underline ubuntu files, and only differs in their own application data.

Shared OS Kernel: All containers running on a host is indeed a bunch of processes with different file systems. They share the same OS kernel, only encapsulates system library and dependencies.

# Docker Features: 

Flexible: Even the most complex applications can be containerised.
Lightweight: Containers leverage and share the host kernel, making them much more efficient in terms of system resources than virtual machines.
Portable: You can build locally, deploy to the cloud, and run anywhere.
Loosely coupled: Containers are highly self-sufficient and encapsulated, allowing you to replace or upgrade one without disrupting others.
Scalable: You can increase and automatically distribute container replicas across a datacenter.
Secure: Containers apply aggressive constraints and isolations to processes without any configuration required on the part of the user.

# Docker Architecture:

Docker uses a client-server architecture. Where a client component talks with the server component using a restful API, over UNIX sockets, or a network interface. All containers on the host share the kernel of the host with respect to the operating system. 

# Fundamental Docker Concepts:

## Docker Essentials:
Platform — the software that makes Docker containers possible
Engine — client-server app (CE or Enterprise)
Client — handles Docker CLI so you can communicate with the Daemon
Daemon — Docker server that manages key things
Volumes — persistent data storage
Registry — remote image storage
Docker Hub — default and largest Docker Registry
Repository — collection of Docker images, e.g. Alpine

Docker Platform is Docker’s software that provides the ability to package and run an application in a container on any Linux server. Docker Platform bundles code files and dependencies. It promotes easy scaling by enabling portability and reproducibility.

Docker Engine is the client-server application. Its is off two types: Docker Community Edition (CE) is free and largely based on open source tools. Docker Enterprise comes with additional support, management, and security features.

Docker Client is the primary way you’ll interact with Docker. When you use the Docker Command Line Interface (CLI) you type a command into your terminal that starts with docker. Docker Client then uses the Docker API to send the command to the Docker Daemon.

Docker Daemon is the Docker server that listens for Docker API requests. The Docker Daemon manages images, containers, networks, and volumes.

Docker Volumes are the best way to store the persistent data that your apps consume and create. 

Docker Registry is the remote location where Docker Images are stored. You push images to a registry and pull images from a registry. You can host your own registry or use a provider’s registry. For example, AWS and Google Cloud have registries.

Docker Hub is the largest registry of Docker images. It’s also the default registry. You can find images and store your own images on Docker Hub for free.

Docker Repository is a collection of Docker images with the same name and different tags. The tag is the identifier for the image.

## Docker Scaling:
Networking — connect containers together
Compose — time saver for multi-container apps
Swarm — orchestrates container deployment
Services — containers in production

Docker Networking allows you to connect Docker containers together. Connected Docker containers could be on the same host or multiple hosts.
Ref: https://www.oreilly.com/content/what-is-docker-networking/

Docker Compose is a tool that makes it easier to run apps that require multiple Docker containers. Docker Compose allows you to move commands into a docker-compose.yml file for reuse. The Docker Compose command line interface (cli) makes it easier to interact with your multi-container app. Docker Compose comes free with your installation of Docker.

Docker Swarm is a product to orchestrate container deployment. 

Docker Services are the different pieces of a distributed app. Docker services allow you to scale containers across multiple Docker Daemons and make Docker Swarms possible.

# How does Docker work:

Docker is based on Linux containers. It uses Linux Kernel features like namespaces and control groups to create containers on top of an operating system.

Docker Engine is the layer on which Docker runs. It’s a lightweight runtime and tooling that manages containers, images, builds, and more. It runs natively on Linux system.

A Docker Daemon that runs in the host computer. actually executes commands sent to the Docker Client — like building, running, and distributing your containers.

A Docker Client that then communicates with the Docker Daemon to execute commands.

A Docker REST API for interacting with the Docker Daemon remotely. 

A Dockerfile is where you write the instructions to build a Docker image.

The Docker image is created at build time and the Docker container is created at run time

# What is a Dockerfile?
Dockerfile is simply a text file written in a specific format, which basically has instructions & arguments that Docker can understand.

Every Docker Image must be based off from another Image. It can be based either on an Operating System or an exiting Image.

Example: 
```
FROM node
Or
RUN npm install
```

# Dockerfile instructions:

FROM — specifies the base (parent) image.
LABEL —provides metadata. Good place to include maintainer info.
ENV — sets a persistent environment variable.
RUN —runs a command and creates an image layer instructs to run a particular command on the image. Used to install packages into containers (software package).
COPY — copies files and directories to the container.
ADD — copies files and directories to the container. Can upack local .tar files.
CMD — provides a command and arguments for an executing container. Parameters can be overridden. There can be only one CMD.
WORKDIR — sets the working directory for the instructions that follow.
ARG — defines a variable to pass to Docker at build-time.
ENTRYPOINT — provides command and arguments for an executing container. Arguments persist.
EXPOSE — exposes a port.
VOLUME — creates a directory mount point to access and store persistent data.

## Example:
RUN apt-get update && apt-get install -y nginx
EXPOSE 8000

# Installing Docker:

## Commands to install docker on Ubuntu:
```
$ sudo apt-get update
$ sudo apt install docker.io
$ docker -v
```
We can push the image to a Docker Hub just like we do in Github and then we can put it into any machine from the Docker hub 
And run it virtually as it contains all the specific dependencies required by the application to run.

## Working with Docker:

mkdir docker-app
cd docker-app
code .

Add the below line in MS vcode
console.log("My First Docker App");

Save as index.js and run as below
node index

Create Dockerfile at the same level where you created index.js file. Copy the below code in Dockerfile and save it

## Example Dockerfile

### Use the official Node.js base image
FROM node:latest
### Copy the package.json and package-lock.json files to the container
COPY package*.json ./index
### Copy the application code to the container
COPY . /index
### Set the working directory inside the container
WORKDIR /index
### Expose the application port
EXPOSE 3000
### Start the Node.js application
CMD ["node", "index.js"]

This Dockerfile sets up a Node.js environment, copies the application files to the container, installs the dependencies, and starts the Node.js application.

### Build the application
docker build -t docker-app .

### Run the application
docker run -d -p 3000:3000 --name node-app-container docker-app

Explanation of options used in the docker run command:

-d: Runs the container in detached mode (in the background).
-p 3000:3000: Maps port 3000 of the host system to port 3000 of the container. 
This is necessary to access the Node.js application running inside the container.
--name node-app-container: Assigns a name to the running container for easy reference.

Access the Node.js Application by visiting http://localhost:3000.

### To push & pull to Hub
docker version
docker login
docker build -t balakarthikeya/docker-app . 
docker push balakarthikeya/docker-app
docker pull balakarthikeya/docker-app
docker run balakarthikeya/docker-app

# Basic Commands

## Create the image
    docker create <image_name>
## Start container
    docker start <container_name>
## Run the images
    docker run <image_name> command
    docker run = docker create + docker start.
## Restarting the exited container.
    docker start -a <container_name>
    "-a" simply means to attach the terminal.
## List images
    docker images  ls
## List all container
    docker ps -all
## List running containers
    docker ps
## How to execute a command in a running container ?
    docker exec -it <service-name-in-compose-file> bash 
## To kill a container?
    docker kill <container_name>
## Stop a specific container
    docker stop <container_name>
## Stop one or more running containers
    docker stop $(docker ps -aq)
## Delete a specific container (only if stopped).
    docker rm <container_name>
## Remove all containers
    docker rm -fv $(docker ps -a -q)
## Remove all images at once
    docker rmi -f $(docker images -q) 
## Show the history of an image
    docker image history	
## Fetch the logs of a container
    docker logs <container-name>
## Remove all volume
    docker volume rm $(docker volume ls -q)
## To delete everything
    docker system prune -a --volumes
## To Build our docker app
    docker build
## Check running container
    docker container ls --all
## Delete a specific image.
    docker image rm <image_name>
## Delete all existing images.
    docker image rm $(docker images -a -q)
## Change a container name at running time.
    docker run --name <container_name> <image_name>
## Display logs of a container.
    docker logs <container_name>
## Create network for container.
    docker network create --driver=bridge --attachable network_name

# Technology Used in Docker:

The programming language used in Docker is GO. Docker takes advantage of various features of Linux kernel like namespaces and cgroups.

namespaces: Docker uses namespaces to provide isolated workspace called containers. When a container is run, docker creates a set of namespaces for it, providing a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

cgroups( Control Groups ): croups are used to limit and isolate the resource usage( CPU, memory, Disk I/O, network etc ) of a collection of processes. cgroups allow Docker engine to share the available hardware resources to containers and optionally enforce limit and constraints.

UnionFS( Union file systems ): are file systems that operate by creating layers, making them very lightweight and fast.It is used by Docker engine to provide the building blocks for containers.

Docker Engine combines the namespaces, cgroups, and UnionFS into a wrapper called a container format. The default container format is libcontainer.

# Install using the Apt repository via Debian

## Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

## Add the repository to Apt sources:
echo "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

## Install the Docker packages
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

## Verify that the installation 
docker run hello-world

## Install using the convenience script
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run

## To Start & Stop & Check Status
systemctl start docker.service
systemctl status docker.service

The default docker path is in /var/lib/docker

# Docker Networking

bridge: used to enable communication between docker containers on the same host internally.

host: uses host system ports for communication.

none : disables communication between docker containers.

overlay: used to enable communication between docker containers on different hosts (swarm nodes).

macvlan: this allows containers to grab a unique IP address from the physical LAN network with DHCP.

# Docker Concepts
Images are read-only templates that you build from a set of instructions written in your Dockerfile.

The Docker image is built using a Dockerfile. Each instruction in the Dockerfile adds a new “layer” to the image, with layers representing a portion of the images file system that either adds to or replaces the layer below it. Layers are key to Docker’s lightweight yet powerful structure. Docker uses a Union File System to achieve this:

The Docker daemon receives the command from the client and manages Docker objects, such as images, containers, networks, and volumes. The Docker client and daemon can either run on the same system, or you can connect a Docker client to a remote Docker daemon. They can communicate using a REST API, over UNIX sockets or a network interface.

The Docker image is created at build time and the Docker container is created at run time.

The Dockerfile is at the heart of Docker. The Dockerfile tells Docker how to build the image that will be used to make containers.

Each Docker image contains a file named Dockerfile with no extension. The Dockerfile is assumed to be in the current working directory when docker build is called to create an image.

Recall that a container is built from a series of layers. Each layer is read only, except the final container layer that sits on top of the others. The Dockerfile tells Docker which layers to add and in which order to add them.

By simply adding Dockerfile that contains instructions that are further used by Docker to package up the application into an image. 

# Union File Systems
Docker uses Union File Systems to build up an image. You can think of a Union File System as a stackable file system, meaning files and directories of separate file systems (known as branches) can be transparently overlaid to form a single file system.