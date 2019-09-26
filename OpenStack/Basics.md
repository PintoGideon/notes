OpenStack is esentially a series of commands known as scripts. Those scripts are bundled into packages called projects
that relay tasks that create cloud environments. In order to create those environments, OpenStack relies on 2 other types of software:

1. Virtualization that creates a layer of virtual resources abstracted from hardware.
2. A base operating system that carries out commands given by OpenStack scripts

Thinks about it like this: OpenStack itself doesn't virtualize resources, but rather uses them to build clouds.
OpenStack also doesn't execute commands, but rather relays them to the base OS. All 3 technologies, OpenStack virtualization, and the base OS- must work together. That interdepenency is why so many OpenStack clouds are deployed using Linux.

### The OpenStack components

![OpenStack](https://user-images.githubusercontent.com/15992276/65698611-cc38ff80-e04a-11e9-8f60-3c44429eeabf.png)



There are 6 stable, core services that handle compute, networking, storage, identity and images while more than a dozen optional ones vary in developmental maturity. Those 6 core services are the infrastructure that allows the rest of the projecs to handle dashboarding, orchestration, bare-metal provisioning, messaging, containers and governance.

### What are Linux Containers

Linux containers are technologies that allow you to package and isloate applications with their entire runtime environments- all of the files necessary to run. This make it easy to move the contained applications between environments while retaining full functionality. Containters are also part of IT security.

Linux containers help reduce conflicts between your development and operation teams by seperating areas of responsibility.
Developers can focus on their apps and operations can focus on the infrastructure.

A Linux container is set of one or more processes that are isolated from th rest of the system. All the files necessary to run them are provided from a distinct image, meaning Linux containers are portable and consistent as they move from development to testing, and finally to production.

You want to emulate those environments as much as possible locally, but without all of the overhead of recreating the server environments. So, how do you make your app work across these environments, pass quality assurance, and get your app deployed without massive headaches, rewriting, and break-fixing? The answer: containers.The container that holds your application has the necessary libraries, dependencies, and files so that you can move it through production without all of the nasty side effects. In fact, the contents of a container image can be thought of as an installation of a Linux distribution because it comes complete with RPM packages, configuration files, etc. But, container image distribution is a lot easier than installing new copies of operating systems. Crisis averted–everyone’s happy.

That’s a common example, but Linux containers can be applied to problems in many different ways where ultimate portability, configurability, and isolation is needed. The point of Linux containers is to develop faster and meet business needs as they arise. In some cases, such as real-time data streaming with Apache Kafka, containers are essential because they're the only way to provide the scalability an application needs. No matter the infrastructure—on-premise, in the cloud, or a hybrid of the two—containers meet the demand. Of course, choosing the right container platform is just as important as the containers themselves.

Container security is multilayered to help protect all parts of the container pipeline.

1. Virtualization let's your operating systems run simulataneously on a single hardware system
2. Containers share the same oerating system kernel and isolate the application processes from the rest of the system.

**_What does this mean?_** For starters, virtualization uses a hypervisor to emulate hardware which allows multiple operating systems to run side by side. This isn’t as lightweight as using containers. When you have finite resources with finite capabilities, you need lightweight apps that can be densely deployed. Linux containers run natively on the operating system, sharing it across all of your containers, so your apps and services stay lightweight and run swiftly in parallel.

### A deeper dive into OpenStack

OpenStack turns hypervisors in a datacenter or across several datacenters, into a pool of resources that can be accessed from a centralized location.

### What is keystone?

- Acts as a common authentication system across the cloud
- Currently supports token-based authorization and user-service authorization
- The identity service generates authentication tokens that permit access to the OpenStack services REST APIs.

### KeyStone Architecture

![keystone](https://user-images.githubusercontent.com/15992276/65698610-cba06900-e04a-11e9-87c7-7a84cad76469.png)

- Uses Python-based library and daemon to receive service and admin API requests

Keystone concepts

1. User- A digital representation of a person , system or service who uses OpenStack services and has assosciated information such as username and password
2. Tenants/ Projects- A container used to group or isolate resources
3. Role- A role includes a set of rights and provileges that specifies what operations a user is permitted to perform in a given tenant/project they are a part of.

![Importance of Keystone](https://user-images.githubusercontent.com/15992276/65698609-cba06900-e04a-11e9-9897-81321f0356c4.png)

### What is Neturon?

The OpenStack Networking service(CodeName-Neturon) is a project focused on delivering Networking as a service (NaaS) between interface devices managed by other OpenStack services.

Networking relies on the identity service for the authentication and authorization of all API requests
Computer interacts with Networking through calls to its standard API, As part of creating a VM, the nova-compute service communicates with the Networking API to plug each Virtual NIC on the VM into a particular network.

The Horizon dashboard integrates with the Networking API, enabling admins and project users to create and manage network services through a web-based GUI.

### Swift Object Storage

![Swift Overview](https://user-images.githubusercontent.com/15992276/65698616-ccd19600-e04a-11e9-8d08-4e4c7e01acfd.png)

Object Storage(Swift) is robust, highly scalable and falut tolerant storage platform for unstructured data such as objects.

The CAP Theorem- Distributed system can not simultaneously guarantee consistency (same view), partition tolerance (Node Access) and Availability(Data Access).
Swift chooses Availability and Partition tolerance over consistency.

The Swift proxy ties the rest of the Swift Architecture and it handles API requests. For each request, it will look up the location of the object, container or object in the ring and route the request accordingly.
We should have a minimum of 2 proxy servers.

Each object must belong to a container when the object belongs to the cluster. From the user's perspective, the object location is where they find the object. Swift object is a binary blob of data like a file

![CAP Theorem](https://user-images.githubusercontent.com/15992276/65698604-cba06900-e04a-11e9-8c4b-2e36d687d0cb.png)


Account---->Many Containers---->Many Objects.

- Core swift components run in controller node.
- Objects are stored in swift object nodes.
- Object Container is a namespace for objects.
- Each object in given container must have a unique name.
- All objects stored in Object Storage have a URL
- All objects stored are replicated 3X in as-unique-as-possible zones, which can be defined as a group of drives, a node, a rack and so on.
- Objects have their own metadata
- **_Developers interact with the object storage system through a RESTful HTTP API_**
- Object data can be located anywhere in the cluster
  Objects can be accessed directly through the swift cli

### Container Properties

1. Container
2. bytes_used
3. account
4. object_count
5. read_acl
6. write_acl

### Container Actions

1. create
2. delete
3. list
4. show
5. set
6. unset
7. save

Swift storage policies enable segmentation of Swift Backend clusters.
All operations are available through the swift client.

### Rings

A ring represents a mapping between the names of entities stored on a disk and their physical locations.

### Zones


![Configuring Zones](https://user-images.githubusercontent.com/15992276/65698606-cba06900-e04a-11e9-9dbb-273f902f43eb.png)


Object storage allows configuring zones in order to isolate failure boundaries.
The goal of zones is to allow the cluster to tolerate significant outtages.

![Partitions](https://user-images.githubusercontent.com/15992276/65698612-cc38ff80-e04a-11e9-838b-fd8e1a631b3d.png)

### Accounts and Containers

Each account and container is an individual SQLite database that is distributed across the cluster. An Account database
contains the list of containers in that account

![Rings](https://user-images.githubusercontent.com/15992276/65698615-cc38ff80-e04a-11e9-9916-f86b41d5b73b.png)

A container database contains the list of objects in that container.

### Setting up a Swift service and Endpoints

Install the swift client

```
apt install swift swift-proxy python-swiftclient

```

There are a few more steps not noted here as I am not going to be running the swift storage.
