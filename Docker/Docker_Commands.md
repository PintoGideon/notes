### Commands
```
docker image -h
docker image ls
```

1. Pulling an image

```
docker image pull nginx
docker image ls
```
The image id for the image is f949e7d76d63 

```
docker image inspect f949e7d76d63 
```
We will get a json document with the details of the image.

```
docker container ls
docker container run -P -d nginx
docker container inspect 1aaade1e957ab59e00f97f65b98576f0bb9c369565b97639728893a08cd869cd
docker container top 1aaade1e957ab59e00f97f65b98576f0bb9c369565b97639728893a08cd869cd
```

The top command displays the running processes of a container.
To start a container, get a list of all the stopped containers

```
docker container ls -a
docker container start container_id
```
The exec command allows us to run a command inside the docker container

```
docker container exec -it c951ccaa8c4a  /bin/bash
localuser@c951ccaa8c4a:~/chris_backend$ ls
check_db_connection.py  collectionjson  config  core  feeds  manage.py  pipelineinstances  pipelines  plugininstances  plugins  uploadedfiles  users
localuser@c951ccaa8c4a:~/chris_backend$ 
```

