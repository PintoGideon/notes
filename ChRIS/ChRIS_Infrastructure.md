# The basics

pfioh- File handling service
pman- Compute service
pfcon- File and compute co-ordinator service.

# Basic Workflow

`pman` manages processes i.e programs or applications that are run by the underlying system. Typically, these processes are command line applications
and usually do not interact really with a user at all. The primary purpose of pman is to provide other software agents the ability to execute processes via http.

### The Barebones

cURL is a tool to do internet transfers for resources specified as URLs using internet protocols.

Networking means communicating between two endpoints on the Internet. The internet is just a bunch of interconnected machines , each using their own private
IP addresses. The addresses each machine have can be of different types and machines can even have temporary addresses. These computers are often called hosts.

When you want to initiate a transfer to one of the machines out there (a server), you usually do not know it's IP addresses but instead you usually know its name. The name of the machine you will talk to is embedded in the URL that you work with when you use curl. Once we know the host name, we need to figure out which IP addresses that host has so that we can contact it. Converting the name to an IP address is often called 'name resolving'.

The name is 'resoved' to a set of addresses. The is usually done by a DNS server. DNS being like a big lookup table that can covert names to addresses. Your computer
normally already knows the address of a computer that runs the DNS server as that is part of setting up the network.

``curl` will therefore ask the DNS server to provide it with all the addresses for 'example.com' and the server will respond with a list of them. With a list of IP addresses for the host curl wants to contact, curl sends out a contact request. The connection curl wants to establish is called TCP and it works sort of like connecting an invisible string between two computers. Once established, it can be used to send a stream of data in both directions.

### Connects to "port numbers"

When connecting with TCP to a remote server, a client selects which port number to do that on. A port number is just a dedicated place for a particular service, which allows that same server to listen to other services on other port numbers at the same time.

When the connecting "string" we call TCP is attached to the remote computer, there is an established connection between the two machines and that connection can then be used to exchange data. Protocal is the language used to ask for data to get send. The protocal describes exactly how to ask the server for data , or to tell the server that the data is coming.

### The Anantomy of URLs

URLs start with a scheme which is the official name for the `http://`. That tells which protocal the URL uses. The scheme identifier is seperated from the rest of the URL by the :// sequence.

The host name part of the URL is ofcourse a name that can be resolved to a numberical IP address. Each protocol has a "default port" that curl will use for it.

Every URL contains a path. The path is sent to the specified server to identify exactly which resource that is requested or that will be provided. The exact use
of the path is protocol independent. For example , getting a file README from the default anonymous user from an FTP server.

```
curl ftp://ftp.example.com/README

```

### Implementation details

```
curl http://example.com

Trying 93.184.216.34

Connected to example.com (93.184.216.34) port 80

```

### Persistent Connections

When setting up TCP connections to sites, curl will keep the old connection around for a while so that if the next transfer is to the same host it can reuse the same connection again and this save a lot of time.

### Client Differences

Curl only gets exactly what you ask it to get and it never parses the actual content-- the data that the server delivers. A browser gets data and it activates different parsers depending on what kind of content it thinks it gets. For example, if the data is HTML it will parse it to display a web page and possibly download other sub resources such as images, Javascript and CSS files. When curl downloads a HTML, it will just get that single HTML resource,even if it, when parsed by a browser would trigger a whole busload of more downloads. If you want curl to download any sub-resource as well, you need to pass those URLs to curl and ask it to get those, just like any other URLs.

### Server Differences

The server that receives the request and delivers data is often setup to act in certain ways depending on what kind of client it thinks communicates with it.

### Intermediaries Findings.

Intermediaries are proxies , explicit or implicit ones. Some environments will force you to use one or you may choose to use one for various reasons, but there are also the transparent ones that will intercept your network traffic silently and proxy it for you no matter what you want.

Proxies are middle men that terminate the traffic and then act on your behalf to the remote server. This can introduce all sorts of explicit filtering and saving you from certain content or even protecting the remote server from what data you try to send to it, but even more so, it introduces another software's view on how the protocol works and what the right things to do are.

### Local Port number

A TCP connection is created between an IP address and a port number in the local end and an IP address and a port number in the remote end. The remote port number can be specified in the URL and usually helps identify which server you are targeting.

### PAC

A proxy is a machine or software that does something for on behalf of you, the client. You can also see it as a middle man that sits between you and the server you want to work with, a middle man that you connect to instead of the actual remote server. You ask the proxy to perform your desired operation for you and then it will runoff and do that and then return the data to you.

Some networks are setup to require a proxy in order for you to reach the Internet or perhaps that special network you are interested in. Some network environmens provides several different proxies that should be used in different situations and a customizable way to handle that is supported by the browsers. A PAC file contains a JS function that decides which proxy a given network connection (URL) should use and even if it should not use a proxy at all. Browsers most typically read the PAC file of a URL on the Local Network.

### HTTP

An HTTP proxy is a proxy that the client speaks HTTP with to get the transfer done. curl will by default assume that a host you point out with -x or --proxy is an HTTP proxy.

```
curl -x 192.168.0.1:8080 http:/example.com/

```

Recall that the proxy receives your request, forwards it to the real server, then reads the response from the server and hands it back to the client.

Most HTTP proxies allow clients to "tunnel through" it to a server on the other side. That's exactly what's done every time you use HTTPS through the HTTP proxy.
You tunnel through an HTTP proxy with curl using -p or --proxytunnel.

If you issue a HTTPS request through a HTTP proxy, it will done by first issuing a connect to the proxy that establishes a tunnel to the remote server and then it sends the request to that server. That fist connect is only issued to the proxy and you may want to make sure only that receives your special header, and send another set of custom headers to the remote sever.

### HTTP

HTTP is a protocol that is easy to learn the basics of, A client connects to a server and it is always the client that takes the initiative. Both the request and response consists of headers and a body. There can be little or a lot of information going in both directions.

An HTTP request sent by a client starts with a request line , followed by headers and then optionally a body. The most common HTTP request is probably the GET request which asks the server to return a speciific resource and this request does not contain a body.

```
GET / HTTP/1.1
User-agent: curl/2000
Host: example.com

```

The server could respond with response headers and a response body

```
HTTP / 1.1 200 Ok
Server: example-server/1.1
Content-Length: 5
Content-Type: plain/text
hello

```

Each HTTP request can be made authenticated. If a server or a proxy wants the user to provide proof that they have the correct credentials to access a URL or perform an action . It can send back a HTTP response code that it informs the client that it needs to provude a correct HTTP authenticaion header in the request to be allowed.

### Docker (Quick bits)

The Ops perspective

When you install Docker, you get two major components:

- The Docker client
- The Docker Daemon

The daemon implements the Docker Engine API.

In the default Linux installation, the client talks to the daemon via a local IPC/UNIX socket at /var/run/docker.sock.

### Images

It's useful to think of a Docker image as an object that contains an OS filesystem and an application. If you work in operations, it's like a virtual machine template. A virtual machine template is essentially a stopped virtual machine. In the Docker world, an image is effectively a stopped container. If you are a developer, you can think of an image as a class.

Getting images in your docker host is called "pulling". The images contain enough of an operating system as well as the code and dependencies to run whatever application it is designed for. Each images gets it's image ID's and it's usually enough just to type the first few characters of the ID.

# Containers

Now that we have an image pulled locally, we can use the `docker container run command` to launch a container from it.

```

$ docker container run -it ubuntu:latest /bin/bash
root@6dc20d508db0

```

The docker container run command `docker container run` tells the Docker Daemon to start a new container. The -it flags tell Docker to make the container interactive and to attach our current shell to the container's terminal.

The format of the docker container exec command is: `docker container exec <options> <container-name or container-id> <command/app>`

### The Dev Perspective

Containers are all about the apps. Here are the steps to containerize a simple application by pulling some source code from Github and building it into an image
using instructions in a Dockerfile.

```

git clone https://github.com/nigelpoulton/psweb.git
cd psweb
ls -l
cat Dockerfile
docker image build -t test:latest


```

### Docker Engine

The Docker engine is the core software that runs and manages containers. We often refer to it simply as Docker, or the Docker platform. As a car engine is made from many specialized parts that work together to make a car drive, the Docker engine is made up from many specialized tools that work together to create and run containers. The Docker Daemon no longer contains any container runtime code. All container runtime code is implemented in a seperate OCI compliant layer. Docker Inc developed their own tool called libcontainer as a replacement for LXC. The goal of libcontainer was to be a platform agnostic tool that provided Docker with access to the fundamental container building blocks that exist inside the kernel.

The Docker Daemon no longer contains any container code. All container code is implemented in a seperate OCI- compliant layer. Docker uses a tool called runc for this, runc is the reference implementation of the OCI container-runtime spec. runc has a single purpose in life- create containers.

containerd is available as a daemon for Linux and Windows and Docker has been using it on Linux since the 1.11 release. In the Docker engine stack, containerd sits between the daemon and run at the OCI layer. Container was originally designed for a single task in life-container life cycle operations. The most common way of starting containers is using the Docker CLI.

The following `docker container run` command will start a simple new container based on the alpine:lastest image.

```
$ docker container run --name ctr1 -it alpine:lastest sh

```

When you type commmands like this into the Docker CLI, the Docker client converts them into the appropriate API payload and POSTs them to the correct API endpoint.

The API is implemented in the Daemon. It is the same rich versioned REST API that has become the hallmark of Docker and is accepted in the industry as the de facto container API.

Once the daemon receives the command to create a new container, it makes a call to containerd. Remember that the daemon no-longer contains any code to create containers. The daemon communicates with the containerd via a CRUD stype API over gRPC. Despite it's name, containerd cannot actually create containers. It uses
runc to do that. It converts the required Docker image into an OCI bundle and tells runc to use this to create a new container.

runc interfaces with the OS kernel to pull together all of the constructs necessary to create a container. The container process is started as a child-process of runc and as soon as it started, runc will exit.

The shim is integral to the implementation of daemonless containers. As we see that containerd uses runc to create new containers. In fact, it forks a new instance of runc for every container it creates. However, once each container is created, its parent runc process exits. This means we can run hundreds of containers without having to run hundreds of run instances.

Once a container's parent run process exits, the assosciated containerd shim process becomes the container's parent. Some of the responsibilities the shim performes as a container's parent include:

` Keeping any STDIN and STDOUT streams open so that when the daemon is restarted, the container doesn't terminate due to pipes being closed etc.

### Images

A Docker image is like a stopped container. If you're a developer you can think of them as being similar to classes. You start by pulling images from the image registry. The most popular registry is Docker Hub, but others do exist. The pull operation downloads the image to your local Docker Host, you can use it to start one or more Docker Containers.

Images are made up of multiple layers that get stacked on top of each other and represented as a single object. Inside of the image is a cut down operating system and all of the files and dependencies required to run an application. Because containers are intended to be fast and lightweight, images need to be small.

Images are considered build time constructs whereas containers are run-time constructs. We use the `docker container run` and `docker service` commands to start one or more containers from a single image. Once you have started a container from an image, the two constructs become independent on each other and you cannot delete the image until the last container using it has been stopped and destroyed.

### Images are usually small.

The whole purpose of a container is to run an application or service. This means that the image a container is created from must contain all OS and application
files required to run the app/service. However containers are all being fast and lightweight. This means that the images they're built from are usually small and stripped of all non-essential parts.

Image registries contain multiple image repositories. In turn, image repositories can contain multiple images.

### Image naming and tagging

Addressing images from official repositories is as simple as giving the repository name and tag seperated by a colon: The format for docker image pull , when working with an image from an official repository is

```
docker image pull <repository> :<tag>
```

```
docker image pull alpine: latest
docker image pull ubuntU: latest

```

These two commands pull the images tagged as "latest" from the "Alpine" and "ubuntu" repositories. The following examples show how to pull various different images from official repositories.

```

$ docker image pull mongo:3.3.11

$ docker image pull redis:latest

$ docker image pull alpine

```

### Searchjng Docker Hub from the CLI

The `docker search command` lets you search Docker from the CLI. You can pattern match against strings in the "NAME" field and filter output based on any of the returned columns. In its simplest form , it searches for all repos containing a certain string in the "NAME field".

```
$ docker search gideonpinto

```

### Images and Layers

A docker image is just a bunch of loosely connected read only layers. Docker takes care of stacking these layers and representing them as a single unified object.

```
$ docker image pull ubuntu:latest

```

Each line in the output above that ends with "Pull complete" representsa a layer in the image that was pulled. All Docker images start with a base layer and as changes are made and new content is added, new layers are added on top.

As an example, lets create a new image based off Ubuntu Linux 16.04. This would be your image's first layer. If you later add the Python package, this would be added as a second layer on top of the base layer. Docker employes a **_storage driver_** that is responsible for stacking layers and presenting them in a single unified file system.

### Sharing image layers

Multiple images can and do share layers. This leads to efficiencies in space and performance. So far we have seen that pulling images by tag is the most common way. But it has a problem, tags are mutable.

Docker introduced a new content addresable storage model. As a part of this model, each image gets a cryptographic content hash. For the purposes of this discussion, we will refer to that hash as the digest. Because the digest is a hash of the contents of the image, it is not possible to change the contents of the image without the digest also changing.

Since Docker version 1.10, an imageis a very loose collection of independent layers. The image itself is really just a configuration object that lists the layers and some metadata. The layers are where the data lives. Each one is fully independent and has no concept of being part of a collective image.

Each image is identified by a crypto ID that is a hash of the config object. Each layer is identified by a crypto ID that is a hash of the content it contains.

This means that changing the content of the image, or any other layers will cause the assosciate crypto hashes to change. As a result, images and layers are immutable and we can easily identify any changes made to either.
