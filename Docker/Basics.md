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
