### Inspecting containers

```sh
docker inspect --format="{{}}"<container hash>

docker run -it -p 80:80 ubuntu:16.04 bash

# Inside the container

apt-get update
apt-get install -y nginx

```

### Docker Volumes

```sh

# In your local computer
mkdir dockertest
touch index.html


docker run --rm -it -p 8011:80 -v /home/username/dockertest:/var/www/html ubuntu:16.04 bash

# Inside the container

apt-get update & apt-get install -y nginx
nginx
/var/www/html

## The index.html from your local computer is now served inside the root folder of nginx

curl localhost:8011
```

### Linking the Full stack

```sh
docker run -d --name=redix redis:alpine

```

Starting a **_mysql_** container.

```sh
docker run -d --name=sql \
-e MYSQL_ROOT_PASSWORD=root \
-e MYSQL_DATABASE=my-app \
-e MYSQL_USER=app-user \
-e MYSQL_PASSWORD=app-pass \
mysql:5.7
```

```sh
docker run -d --name=php \
--link=redis:redis \
--link=mysql:mysql \
-v $(pwd)/application: /var/www/html \
shippingdocker/php:0.1.0
```

```sh
docker run -d --name=nginx \
--link=php:php \
-p 80:80 \
-v $(pwd)/application:/var/www/html  \
shippingdocker/nginx:0.2.0
```

### Docker Network

Docker network lets us create a network and add containers to the network.If we name the containers, the docker network will setup the DNS and we can resolve the ip addresses of the containers using the containers name.

```sh
docker network ls
docker network create --driver=bridge <network_name>

```

### Docker volumes

We can share volumes between our host machine and the container.

```sh
docker volume ls
```
If we go to the msql image on docker hub and inspect the Dockerfile, we see a volume command.

This means that when the container is spun off, create a named volume and share it to the /var/lib/mysql directory inside of the container.

```sh
VOLUME /var/lib/msql
```

### Exploring the `mysql` repository

Envionment Variables:
When you start the `mysql` image, you can adjust the configuration of the MySQL instance by passing one or more environment variables on the
```docker run``` comand.


