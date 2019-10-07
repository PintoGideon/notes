### Docker Architecture

- Client-Server Architecture
- The Client talks to the Docker daemon
- The Docker Daemon handles building,running,distributing

Both the Daemon and the Client communicate using the REST api.

- UNIX sockets
- Network interface.

The Docker Dameon (dockerd):

Listens for Docker API requests and manages Docker objects:

1. Images
2. Containers
3. Networks
4. Volumes

The Docker client (docker)

- is how users interact with Docker
- The client sends commands to dockerd

Docker registries

- Stores Docker images
- Public registry such as DockerHub

### How to create a docker image

Docker images:

- Use images to create an instance of a container
- Comprised of multiple layers
- Build Time constructs
- Build from the instructions

Create an onboarding image.

```
mkdir onboarding
cd onboarding/
vim dockerfile
```

In the dockerfile

```
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install -y python3
```

```
docker build .
```

```
docker container run hello-world
```

1. The Docker client contacted the Docker daemon.
2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
   (amd64)
3. The Docker daemon created a new container from that image which runs the
   executable that produces the output you are currently reading.
4. The Docker daemon streamed that output to the Docker client, which sent it
   to your terminal.
   (Elaborate explaination below)

### Docker objects

- Images
  Read-only template with instructions for creating a Docker container
  Image is based on another image
  Create your own images
  Use a Dockerfile to build images

- Containers
  Running instance of an image
  Connect a container to networks
  Attach storage
  Create a new image based on it's current state
  Isolate from other containers and other hosts

- Services
  Scale containers across multiple Docker daemons
  Docker Swarm
  Define the desired state
  Service is load-balanced

### Docker swarm

Multiple Docker Daemon(Master and Workers)
The daemons communicate with each othe using the Docker API.

### Docker engine

Modular in design

- Batteries included but replaceable

Components

- Docker Client
- Docker daemon
- containerd
- runc

runc

- Implementation of the OCI container-runtime-spec
- Lightweight CLI wrapper for libcontainer
- Create containers

containerd

- Manage container life cycle

- Stop
- Start
- Pause
- Delete
  Image management

### Running Containers

```
docker container run -it --name <NAME><IMAGE>:<TAG>
```

Creating a container:

- Use the CLI to execute a command
- Docker client uses the appropriate API payload
- POSTs to the correct API endpoint
- The Docker Daemon receives instructions
- The Docker Daemon calls containerd to start a new container
- The Docker Daemon uses gRPC(a CRUD style App)
- containerd creates an OCI bundle from the Docker image
- Tells runc to create a container using the OCI bundle
- runc interfaces with the OS kernel to get the constructions to create a container

### Creating Containers

```
docker container run busybox
docker container ls -a
```

The output looks like this:

```

CONTAINER ID        IMAGE                          COMMAND                  CREATED             STATUS                      PORTS                              NAMES
7a5bf7b89acd        busybox                        "sh"                     10 seconds ago      Exited (0) 7 seconds ago                                       jovial_tu
```

When the container starts, it's going to go and execute the command sh. That command is going to execute and quit. It's not going to execute as a task and not a process.

### Exposing container ports

```
docker container run container_id
docker container inspect 2d3c59d2d8f2
```

Export a port or a range of ports

```
docker container run -d --expose 3000 postgres
1c2bda347e8ebbfa7543f9b0f4ae336484ea2a8154a1d0eb19b30b4cc12203fe
docker container ls
CONTAINER ID        IMAGE                         COMMAND                  CREATED             STATUS              PORTS                              NAMES
1c2bda347e8e        postgres                      "docker-entrypoint.s…"   9 seconds ago       Up 7 seconds        3000/tcp, 5432/tcp                 recursing_kapitsa


```

Mapping a container's port to a host's port

```
docker container run -d --expost 3000 -p 80:3000 nginx
7dcb182c1604        nginx                         "nginx -g 'daemon of…"   7 seconds ago       Up 4 seconds        80/tcp, 0.0.0.0:81->3000/tcp       agitated_jennings
```

Listing all port mappings or a specific mapping for a container

```
docker container port <name>

```

### Executing Container commands

```
docker container run container_id
docker container run exec it nginx /bin/bash

```

### What is a Dockerfile

Dockerfiles are instructions on how to build an image

The file contains all the commands used to start a container.

1. Docker image consists of read-only layers.
2. Each Layer represents a Dockerfile instruction.
3. Layers are stacked
4. Each layer is a delta of the changes from the previous layer
5. Images are built using the docker image build command

### Dockerfile

```
FROM ubuntu.15.04
COPY ./app
RUN make /app
CMD python /app/app.y


```

### Layers:

1. FROM creates a layer from the ubuntu:15.04 Docker image
2. COPY adds files from your Docker client's current directory
3. RUN builds your application with make
4. CMD specifies what command to run with the container.
5.

### General guidelines:

1. Keep your containers as ephemeral as possible
2. Avoid including unnecessary file
3. Use .dockerignore
   4 Use multi stage builds
4. Don't install unnecessary packages
5. Decouple applications
6. Sort multi-line arguments
7. Leverage the build cache

### Working with instructions

```
FROM node
LABEL org.label-schema.version=v1.1
RUN mkdir -p/var/node/
ADD src/ /var/node/
WORKDIR /var/node
RUN npm install
EXPOSE 3000
CMD ./bin/www
~
```

FROM- Initializes a new build stage and sets the Base image
RUN- Will execute any commands in a new layer
CMD- Provides a default for an executing container. There can be only one CMD instruction in a Dockerfile.
LABEL: Adds metadata to an image
EXPOSE: Informs Docker that the container listens to the specified network ports at runtime
ENV: Sets the environment variable <key> to the value <value>
ADD: Copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at path <dest>
WORKDIR: Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

```
docker image build -t linuxacademy/weather-app:v1 .
docker image ls
docker container run -d --name=weather-app1 -p 8081:3000 linuxacademy/weather-app:v1

```
