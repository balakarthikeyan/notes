# What is Docker?
Docker is a set of platform as a service product that use OS-level virtualisation to deliver software in packages called containers. 
By using Docker, developers can build, pack, share, deploy and run applications as lightweight, portable, self-sufficient containers and running virtually anywhere.

# What is a Docker Image?
In Docker, An image is a combination of a file system and parameters. It is a single file with all the dependencies and configurations required to run a program.

# What is a Container?
A container is an instance of an image, which is a running program. Containers allow developers to package an application with its dependencies and deploy it as a single unit. Docker is used to automate the deployment of software in lightweight containers. The use of containers to deploy applications is called `containerisation`. It packages applications as images that contain code, runtime environment, libraries, and configuration.

Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. All containers are run by a single operating-system kernel and are thus more lightweight than virtual machines.

# What is a Dockerfile?
Dockerfile is simply a text file written in a specific format, which basically has instructions & arguments that Docker can understand.

Every Docker Image must be based off from another Image. It can be based either on an Operating System or an exiting Image.

The `Dockerfile` is at the heart of Docker.
The `Dockerfile` tells Docker how to build the image that will be used to make containers.
The `Dockerfile` tells Docker which layers to add and in which order to add them.
The `Dockerfile` is assumed to be in the current working directory when `docker build` is called to create an image.
The `Dockerfile` that contains instructions that are further used by Docker to package up the application into an image. 

Example: 
```bash
FROM node
# Or
RUN npm install
```

# Dockerfile instructions:

- `FROM:` specifies the base (parent) image.
- `LABEL:` provides metadata. Good place to include maintainer info.
- `ENV:` sets a persistent environment variable.
- `RUN:` runs a command and creates an image layer instructs to run a particular command on the image. Used to install packages into containers (software package).
- `COPY:` copies files and directories to the container.
- `ADD:` copies files and directories to the container. Can upack local .tar files.
- `CMD:` provides a command and arguments for an executing container. Parameters can be overridden. There can be only one CMD.
- `WORKDIR:` sets the working directory for the instructions that follow.
- `ARG:` defines a variable to pass to Docker at build-time.
- `ENTRYPOINT:` provides command and arguments for an executing container. Arguments persist. To configure a container that will run as an executable
- `EXPOSE:` exposes a port and informs Docker that the container listens on the specified network ports at runtime.
- `VOLUME:` creates a directory mount point to access and store persistent data.

## Example:
```bash
RUN apt-get update && apt-get install -y nginx
EXPOSE 8000
```

# Installing Docker:

## Commands to install docker on Ubuntu:
```bash
$ sudo apt-get update
$ sudo apt install docker.io
$ docker -v
```

## Working with Docker Hub:
```bash
mkdir docker-app
cd docker-app
code .

# Add the below line in MS vcode
console.log("My First Docker App");

# Save as index.js and run as below
node index
```

Create Dockerfile at the same level where you created index.js file. Copy the below code in Dockerfile and save it

## Example Dockerfile

```bash
# Use the official Node.js base image
FROM node:latest
# Copy the package.json and package-lock.json files to the container
COPY package*.json ./index
# Copy the application code to the container
COPY . /index
# Set the working directory inside the container
WORKDIR /index
# Expose the application port
EXPOSE 3000
# Start the Node.js application
CMD ["node", "index.js"]
```

This Dockerfile sets up a Node.js environment, copies the application files to the container, installs the dependencies, and starts the Node.js application.

## Build the application
`docker build -t docker-app .`

## Run the application
`docker run -d -p 3000:3000 --name node-app-container docker-app`

- `-t:` specifies tagname, default is latest 
- `-d:` Runs the container in detached mode (in the background).
- `-p 3000:3000:` Maps port 3000 of the host system to port 3000 of the container. 
- `--name node-app-container:` Assigns a name to the running container for easy reference.

Access the Node.js Application by visiting `http://localhost:3000.`

## To push & pull to Hub
```bash
docker version
docker login
docker build -t balakarthikeya/docker-app . 
docker push balakarthikeya/docker-app
docker pull balakarthikeya/docker-app
docker run balakarthikeya/docker-app

# Now create a container from image
docker run -it --name my-ubuntu ubuntu /bin/bash

# Now create image of existing container
docker commit <container-id> <image-name>

docker tag local-image-name username/repository:tag
docker push username/repository:tag
```

# Basic Commands
## Create the image
    docker create <image_name>
## Start container
    docker start <container_name>
## Run the images
    docker run <image_name> command
    docker run = docker create + docker start.
## Run the application
    docker run -d --name <container_name> -p {external_port}:{internal_port} <image_name>
## Restarting the exited container.
    docker start <container_name>
## List running containers
    docker container list
    docker container ps
    docker ps
## List all container
    docker container ls --all
    docker ps --all
## List all images
    docker images
    or
    docker image ls
    docker images --filter "reference=php*"
## To execute commands on running containers
    docker exec <options> <container_name> <command>

    Examples:
    docker exec -u 0 <container_name> whoami
    docker exec -w /var/log 74f86665f0fd cat dmesg
    docker exec -e UID='myuser' 74f86665f0fd printenv UID
## How to execute a command in a running container ?
    docker exec -it <service-name-in-compose-file> bash

    Options:
    `-i` option, binding the standard input (stdin) of your host to the stdin of the process you are running in the container.
    `-t` option, allocates a pseudo-TTY
    `-w` option, specify the working directory to execute the command.
    `-e` option, and specify the environment variable name and value next to it.
    `-u` option, with a value of 0 for the root user.
    `-d` option, detached mode: run command in the background.
## Kill a specific container
    docker kill <container_name>
## Kill one or more running containers
    docker kill $(docker ps -a -q)
## Stop a specific container
    docker stop <container_name>
## Stop one or more running containers
    docker stop $(docker ps -a -q)
## Delete a specific container (only if stopped).
    docker rm <container_name>
## Delete all containers
    docker rm -f $(docker ps -a -q)
## Delete all volumes
    docker rm -v $(docker ps -a -q)
## Remove all images at once
    docker image remove -f $(docker images -a -q)
    or
    docker rmi -f $(docker images -a -q) 
## Show the history of an image
    docker image history	
## Fetch the logs of a container
    docker logs <container-name>
## Remove all volume
    docker volume rm $(docker volume ls -q)
## To Build our docker app
    docker build
## Delete a specific image.
    docker image rm <image_name>
## Delete all existing images.
    docker image rm $(docker images -a -q)
## Change a container name at running time.
    docker run --name <container_name> <image_name>
## Display logs of a container.
    docker logs <container_name>
## Create network for container.
    docker network create --driver=bridge --attachable <network_name>
## Check docker disk space usage 
    docker stats
    or
    docker system df
## Removing unused containers
    docker ps --filter status=exited --filter status=dead -q
## Remove build cache
    docker builder prune
## Remove all stopped containers
    docker container prune
## Removing dangling images
    docker images --filter dangling=true -q
## Remove unused images
    docker image prune
## Removing all docker volumes
    docker volume prune
## Removing networks
    docker network prune
## To remove containers, images and networks use:
    docker system prune
## To remove containers, images, networks and volumes, use
    docker system prune --volumes
## To list all images with their repository and tag in a table format
    docker images --format "{{.ID}} : {{.Repository}} : {{.Tag}}"

|   Option  |   Description |
|   ------  |   ----------- |
|   -a, --all		|   Show all containers |
|   --filter		|   Provide filter values with format is of "key=value"   |
|   -f, --force		|   Do not prompt for confirmation  |
|   -q, --quiet		|   Only display container IDs  |
|   -s, --size		|   Display total file sizes    |

Docker containers have a `status` field indicating where they are at in their lifecycle. `status` can be one of `created, restarting, running, removing, paused, exited, or dead.`

# Install Docker on your Local Machine via Ubuntu/Debian

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
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo "deb [arch="$(dpkg --print-architecture)" signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker engine
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start and enable Docker service
sudo systemctl start docker
sudo systemctl enable docker

# Add your user to the 'docker' group to run Docker commands without sudo (Optional)
sudo usermod -aG docker $USER
```

## Verify that the installation 
`docker run hello-world`

## Install using the convenience script
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
```

The default docker path is in /var/lib/docker

# Docker Compose Commands
```bash
docker-compose --env-file=.env.local up -d
docker-compose logs -f
docker compose build --pull --no-cache
docker-compose up -d --build
docker compose run <service-name-in-compose-file> composer install
```