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
