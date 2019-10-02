Docker Networking:

1. Container Network Model (CNM)
2. The libnetwork implements CNM
3. Drivers extends the modek by network topologies

Network drivers:

- bridge
- host
- overlay
- macvlan
- none
- Network plugins

Container Network Model

Defines three building blocks:

- Sandboxes
- Endpoints
- Networks

```
ifconfig


```

This is going to show all our network adapters. The first adapter is docker0 that is going to bind itself to eth0 and loopback.

```
docker network -h
docker network ls
docker network inspect a73f6f7ac872

```

### Creat a network

```
docker network create <Name>
docker network inspect br00
docker network rm br00
```

### Storage overview

Categories of data storage:
Non-persistent

1. Data that is ephemeral
2. Every Container has it
3. Tied to the lifecycle of the container

Persistent

1.  Volumes
2.  Volumes are decoupled from containers

Storage locations on linux: /var/lib/docker/<Storage-Driver>/

Volumes:
Use a volume for persistent data

1.  Create the volume
2.  Create your container

```
docker volume -h
docker volume inspect volume_name

```

```
[
    {
        "CreatedAt": "2019-10-01T13:34:59-04:00",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "chris_ultron_backend",
            "com.docker.compose.version": "1.24.0",
            "com.docker.compose.volume": "swift_storage"
        },
        "Mountpoint": "/var/lib/docker/volumes/chris_ultron_backend_swift_storage/_data",
        "Name": "chris_ultron_backend_swift_storage",
        "Options": null,
        "Scope": "local"
    }
]
```
