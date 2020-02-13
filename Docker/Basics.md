### Docker images without Docker

Reference link: https://btholt.github.io/complete-intro-to-containers

Images:

These are pre-made containers called images. THey basically dump out the state of the container, package that up and store it so you
can use it later. Docker does a lot more for you than just this like networking, volumes, and other things but suffice to say this core of what Docker is doing for you: creating a new environment that's isolated by namespace and limited by cgroups and chroot'ing you into it.

Tags:

So far we've just been running containers with random tags that I chose. If you run docker run -it node the tag implicitly is using the latest tag. When you say docker run -it node, it's the same as saying docker run -it node:latest. The :latest is the tag. This allows you to run different versions of the same container, just like you can install React version 15 or React version 16: some times you don't want the latest. Let's say you have a legacy application at your job and it depends on running on Node.js 8 (update your app, Node.js is already past end-of-life) then you can say

```
docker run -it node:8 bash
```

### Kill all the containers at once

```
docker kill(docker ps -q)
```

**_docker exec is going to run an existing container while docker run is going to run a new container_**.

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

- FROM- Initializes a new build stage and sets the Base image
- RUN- Will execute any commands in a new layer
- CMD- Provides a default for an executing container. There can be only one CMD instruction in a Dockerfile.
- LABEL: Adds metadata to an image
- EXPOSE: Informs Docker that the container listens to the specified network ports at runtime
- ENV: Sets the environment variable <key> to the value <value>
- ADD: Copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at path <dest>
- WORKDIR: Sets the working directory for any RUN, CMD, - - ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

```
docker image build -t linuxacademy/weather-app:v1 .
docker image ls
docker container run -d --name=weather-app1 -p 8081:3000 linuxacademy/weather-app:v1

```

### Alpine Linux

Making your containers smaller is a good thing for a few reasons. For one, everything tends to get a bit cheaper. Moving containers across the Internet takes time and bits to do. If you can make those containers smaller, things will go faster and you'll require less space on your servers. Often private container registries (like personal Docker Hubs, Azure Container Registry is a good example) charge you by how much storage you're using.

### Docker Engine

The Docker Daemon was a monolithic binary. It contained all of the code for the docker client, the Docker API , the containe runtime, image builds and much more.

LXC provide the Daemon with access to fundamental building blocks of containers that existed in the Linux Kernel
Things like cgroups and namespaces.

LXC is linux specific. This is was a problem.

Libcontainer replaced LXC as the default execution driver in Docker 0.9 which is a platform agnostic tool that provided Docker with access to the fundamental container building blocks that exist inside the Kernel.

### A quick workflow for mental models.

```
docker container run --name ctr1 -it alpine:latest sh
```

When you type these commands into the Docker CLI, the Docker client converts them into the appropriate API payload and POSTs them to the correct API enpoint.

THe API is implemented in the Daemon. The daemon communicates with containerd via a CRUD style API. It converts the required Docker image into an OCI bundle and tells runc to use
this to create a new container. runc interfaces with the OS kernel to pull together all of the constructs necessary to
create a container (namespaces, cgroups etc.). The container process is started as a
child-process of runc, and as soon as it is started runc will exit.

all container runtime code is implemented in a separate
OCI-compliant layer. By default, Docker uses a tool called runc for this. runc is
the reference implementation of the OCI container-runtime-spec. This is the runc
container runtime layer in Figure 5.3. A goal of the runc project be in-line with the
OCI spec.

### Images

The whole purpose of a container is to run an application or service. The means that the image, a container is created from must contain all OS and application files.

All containers running on a Docker host share access to the Host's kernel.

```
docker image pull <repository>:<tag>
```

```
docker image pull ubuntu:latest
```

This command pulls the image tagged as latest from the ubuntu repo.
A Docker image is just a bunch of loosely connected read only layers.

### Multi architecture Images.

The manifest list is a list of architectures supported by a particular image tag. Suppose you are running Docker on Raspberry Pi, When we pull an image, your Docker client makes the relevant calls to the Docker registry API running on Docker Hub. If a manifest list exists for the image, it will be parsed to see if an entry exists for Linux on ARM. If an ARM entry exists, the manifest for that image is retrieved and parsed for the crypto ID’s of the
layers that make up the image. Each layer is then pulled from Docker Hub’s blob
store.

```
$ docker container run --rm golang go version
```

### Containers

A container is the runtime instance of an image. In the same way that we can start a virtual machine from a VM template, we can started one or more containers from a single image.

```
docker container run <image> <app>
```

### VM model

In a VM mode, the physical server is powered on and the hypervisor boots. Once the hypervisor boots it lays claim on all your physical resources such as CPU, RAM, storage and NICs. The hypervisor then carves these hardware resources into virtual versions that looks smell and feel exactly like the real thing. It then packages them into a software construct called a VM.

### Container Model.

When the server is powered on, your chosen OS boots. In the Docker world this can be Linux, or a modern version of Windows that has support for the container primitives in its kernel. Similar to the VM model, the OS claims all hardware
resources. On top of the OS, we install a container engine such as Docker. The container engine then takes OS resources such as the process tree, the filesystem, and the network stack, and carves them up into secure isolated constructs called
containers. Each container looks smells and feels just like a real OS. Inside of each container we can run an application. Like before, we’re assuming a single physical server with 4 applications

Containers cannot exist without a running process. Ctrl-PQ exits the container without killing it.
Volumes are the preferred way to store persistent data in containers.

```
docker container run -d --name webserver -p 809:8080 nigelpoulton/pluralsight-docker-ci
```

-d stands for daemon mode and tells the container to run in the background.
port mappings are expressed as host-port:container-port.

### Dockerfile

Dockerfile has 2 main purposes:

1. To describe the application
2. To tell Docker how to containerize the application (create an image with the app inside.)

The Dockerfile uses the WORKDIR instruction to set the working directory for the rest of the instructions in the file.

### Pushing images.

Before you can push an image you need to tag it in a special way.

1. Registry
2. Respository
3. Tag

I don’t have access to the web repository, all of my images have to sit within the gideonp second-level namespace. This means
we need to re-tag the image to include my Docker ID.

```
$ docker image tag web:latest gideonp/web:latest
$ docker image push gideonp/web:latest

```

The docker image build command parses the Dockerfile one-line-at-a-time starting from the top. Some instructions create new layers whereas others add metadata to the image. Examples that create new layers are RUN,COPY, FROM. Examples of instructions that create metadata include EXPOSE, WORKDIR, ENV and ENTRYPOINT. If an instruction is adding content such as files and programs to the image, it will create a new layer. If it is adding instructions on how to build the image and run the application, it will create metadata.

```
docker image history web:latest
docker image inspect web:latest
```

The basic process is.

1. Spin up a temporary container
2. Run the Dockerfile instruction inside of that container
3. Save the results as a new image layer
4. Remove the temporary container.

### Moving to production with multistate builds

When it comes to Docker images, big is bad.

We can have a single Dockefile containing multiple FROM instructions. Each FROM instruction is a new build stage that can easily COPY artifacts from previous stages.

Some key things to remember:
As soon as any isntruction results in a cache-miss, the cache is no longer used for the rest of the entire build.
A good pattern would be try and build our images in a way that places any instructions that are likely to change towards the end of the file.

Summary:

1. docker image build is the command that reads a Dockerfile and containerizes an application. The -t flags the image and the -f flag lets you specify the name and location of the Dockefile.

The build context is where you application files exist and this can be a directory on your local Docker host or a remote Git repo

a. The FROM instructions in a Dockerfile specifies the base image for the new image you will build.
b. The RUN instruction in a Dockerfile allows you to run commands inside the image which creates new layers.
c. The COPY instruction in a Dockerfile adds files to the image as a new layer. Common use is to COPY your application code into an image.
d. The ENTRYPOINT instruction in a Dockerfile sets the default app to run when the image is started as a container.

### Deploying apps with Docker Compose.

Most modern apps are made of multiple smaller services that interact to form a useful app. We call this microservices. A simper example might be an app with the following four services.

1. Web Front end
2. ordering
3. Catalog
4. backend database

Deploying and managing a lot of services can be tricky. Docker compose lets you describe an entire app in a single declarative config file. Once the app is deployed , you can manage its entire lifecycle with a simple set of commands.

docker compose is an external tool that gets bolted on top of the Docker engine. Compose is still an external Python binary that you to install on host running the docker engine. You define multi-container (multi-
service) apps in a YAML file, pass the YAML file to the docker-compose binary, and
Compose deploys it via the Docker Engine API.

### Compose Files

The default name for compose YAML file is docker-compose.yml.

The services key is where we define different application services. For example an application can define two service; a web front-end called web-fe and in memory database called redis. Compose will deploy each of these services as its own container.

The top-level networks key tells Docker to create new networks. Compose will create bridge networks. These are single host networks that can only connect containers on the same host.

The top-level volumes key is where we tell Docker to create new volumes.

```docker
version: "3.5"
services:
    web-fe:
      build: .
      command: python app.py
      ports:
        -target:5000
         published:5000
      networks:
        -counter-net
      volumes:
        -type:volume
         source:counter-vol
         target: /code

     redis:
       image: "redis-alpine"
       networks: counter-net

networks:
     counter-net

volumes:
     counter-vol
```

```
docker-compose up &
```

Once we have updated the app, we need to copy it into the volume on the Docker host. Each Docker volume is exposed at a location with the Docker host's filesystem, as well as a mount point in or more containers. Use the following command to find where the volume is exposed on the Docker host.

```
$ docker volume inspect counter-app-vol | grep Mount

```

### Docker Swarm

At a high level swarm has two major components.

1. A secure cluster
2. An orchestration engine

Docker Swarm is two things: An enterprise grade secure cluster of Docker Hosts and an engine for orchestrating microservices apps.

On the orchestration front, swarm exposes a rich API that allows you to deploy and manage complicated microservices. You can define your apps in declarative manifest files and deploy them with native Docker commands.

Docker swarm is fully integrated into the Docker engine. Swarm consists of one or more nodes. The only requirement is that all the nodes can communicate over reliable networks.

Nodes are configures as managers or workers. Managers looks after the control plan of the cluster, meaning things like the state of the cluster and dispatching tasks to workers. Workers accepts tasks from managers and execute them.
omething that’s game changing on the clustering front is the approach to security.
TLS is so tightly integrated that it’s impossible to build a swarm without it. In today’s security conscious world, things like this deserve all the props they get! Anyway, swarm uses TLS to encrypt communications, authenticate nodes, and authorize roles.
Automatic key rotation is also thrown in as the icing on the cake! And it all happens
so smoothly that you wouldn’t even know it was there!

Running docker swarm init on a Docker host in a single engine mode will switch that node into swarm mode, create a new swarm and make the node the first manager of the swarm. Additional nodes can be joined as workers and managers. This obviously switches them into swarm mode as part of the operation.

### Docker Networking

Docker runs applications inside of containers, and these need to communicate over lots of different networks. This means Docker needs strong networking capabilities.

Docker networking is based on open source pluggable architecture called CNM (Container Network Model).
To create a smooth out-of-the-box experience, Docker ships with a set of native drivers that deal with the most common networking requirements. These include single-host bridge networks, multi-host overlays, and options for plugging into
existing VLANs. Ecosystem partners extend things even further by providing their
own drivers.
Last but not least, libnetwork provides a native service discovery and basic container load balancing solution.

CNM is the design spec. libenetwork is a real-world implementation of the CNM. It is written in Go.

1. Sandboxes
2. Endpoints
3. Networks

sandbox is an isolate network stack. It includes Ethernets, interfaces, ports , routing tables and DNS config.
Endpoints are virtual network interfaces. Like normal network interfaces, they are responsible for making connections.
Networks are a software implementation of the 802.1d bridge. The group together and isolate, a collection of endpoints that need to communicate.

Nowadays, all of the core Docker networking lives in libnetwork.

Single-host bridge networks:
The simplest type of Docker network is the single host bridge network.

1. Single-host tells us it only exists on a single Docker host and can only connect containers that are on the same host.
2. Bridge tells us that it's an implementation of an 802.1d bridge

Every Docker host gets a default single-host bridge network. THe bridge network maps to the docker0 linux bridge in the host's kernel which can be mapped back to an Ethernet interface on the host via port mappings.

```
docker network create -d bridge localnet
```

The new network is created, and will appear in the output of any future docker216network ls commands. If you are using Linux, you will also have a new Linux bridge created in the kernel.
Let’s use the Linux brctl tool to look at the Linux bridges currently on the system. You may have to manually install the brctl binary using apt-get install bridge- utils , or the equivalent for your Linux distro.

If we add another new container to the same network, it should be able to ping the
“c1” container by name. This is because all new containers are registered with the
embedded Docker DNS service so can resolve the names of all other containers on
the same network.

### Docker Volumes

There are two main categories of data. Persistent and non-persistent.

Persistent is the stuff you need to keep. Things like customer records, financials, bookings, audit logs and even some types of application log data. Non-persistent is the stuff you don't need to keep.

Every Docker container gets its own non-persistent storage. It's automatically created, alongside its container and it's tied to the lifecycle of the container.

If you want your container's data to stick around, you need to put it in a volume.

At a high level, you create a volume, then you create a container, and you mount the volume into it. The volumne gets mounted to a directory in the container's filesystem and anything written to that directory is written to the volume.

Using the following command , we can create a new volume.

```bash
$docker volume create myvol
```

By default , Docker creates new volumes with the built-in local driver. Third-party drivers are available as plugins.
These can provide advanced storage features, and integreate external storage systems with Docker.

```bash
$ docker volume inspect myvol
[
{
"CreatedAt": "2018-01-12T12:12:10Z",
"Driver": "local",
"Labels": {},
"Mountpoint": "/var/lib/docker/volumes/myvol/_data",
"Name": "myvol",
"Options": {},
"Scope": "local"
}
]
```

Here the driver and scope are both local. This means the volume was created with the default local driver.

### Playing with Volumes

The following command creates a new standalone container and mounts a volume called bixvol.

In the docker-compose.dev.yml file, volumes are written like thie following:

```bash
volumes:
- type: volume
source: counter-vol
target: /code
```



```bash

docker container run --dit --name voltainer --mount source=bizvol, target=/vol alpine

```

If you specify an existing volume, Docker will use the existing volume, if it does not exist, Docker will create it for you.

### Sharing storage across cluster nodes

Integrating Docker with external storage systems makes it easy to share the external storage between cluster nodes.
For example, a single storage LUN or NFS share can be presented to multiple Docker hosts, and therefore made available to containers.


### Some Images for mental models:

![docer-port-map](https://user-images.githubusercontent.com/15992276/74468606-16d7de80-4e69-11ea-9841-591b8f35fafb.png)
![docker0](https://user-images.githubusercontent.com/15992276/74468608-16d7de80-4e69-11ea-9cfe-bcbbb849c491.png)
![Docker](https://user-images.githubusercontent.com/15992276/74468609-17707500-4e69-11ea-9a4e-1e7f6f7a23ba.png)
![docker_compose](https://user-images.githubusercontent.com/15992276/74468610-17707500-4e69-11ea-933e-fc8474a19847.png)
![docker_network](https://user-images.githubusercontent.com/15992276/74468611-17707500-4e69-11ea-9060-dfbf6ce4d6d0.png)
![docker_volumes](https://user-images.githubusercontent.com/15992276/74468613-17707500-4e69-11ea-8448-2ac600fb7c77.png)
