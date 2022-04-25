## pull image and latest version
``` 
docker pull mongoclient/mongoclient:4.0.0 
```

## run it container as a daemon
```
docker run -d -p 3000:3000 mongoclient/mongoclient
```

## run container mongodb and persist data
```
docker run -d -p 3000:3000 -v C:\televida\local-environment\nosql\data_nosql:/data/db mongoclient/mongoclient

```

## Dockeer build image
```
docker build -t televida-activemq:5.9.0 .
```

## Docker rebuild image
```
docker build -t --force-rm televida-tomcat:9.0.36 .
```

## Rebuild container
```
docker-compose up -d --force-recreate --no-deps --build container
```

## Docker run new container
```
docker run --name televida-chat-gofitness -p 8085:80 -d televida-chat-web
```

## Access bash container
```
docker exec -it transportdb bash
```

## Access bash container and available tty
```
docker exec -it --tty chat_tomcat-api_1 bash
```

## Access bash container with permission root user
```
docker exec -it --user root transportapp bash
```

## Full script for rebuilding a Docker container    
According to this new knowledge, I fixed my script in the following way:
```
#!/bin/bash
imageName=xx:my-image
containerName=my-container

docker build -t $imageName -f Dockerfile  .

echo Delete old container...
docker rm -f $containerName

echo Run new container...
docker run -d -p 5000:5000 --name $containerName $imageName
```

## List stopped containeres
```
docker ps -a
```

## stopped containeres
```
docker stop <name/id>
```

## Start docker container exited
```
docker start -i <name/id>
```


## Remove images by id
```
docker image rm ae21c72df409
```


## Removing All Unused Docker Objects
```
docker system prune  
```


You’ll be prompt you to confirm the operation:
WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all build cache
Are you sure you want to continue? [y/N]

If you want to remove all unused images not just the dangling ones, add the -a (--all) option to the command:
```
docker system prune -a
```

By default, the command doesn’t remove unused volumes to prevent losing important data. To remove all unused volumes, pass the --volumes option:
```
docker system prune --volumes
```

## Removing Docker Containers
Docker containers are not automatically removed when you stop them unless you start the container using the --rm flag.

### Removing one or more containers
To remove one or more Docker containers, use the docker container rm command, followed by the IDs of the containers you want to remove.

You can get a list of all containers by invoking the docker container ls command with the -a option:
```
docker container ls -a
```

Once you know the CONTAINER ID of the containers you want to delete, pass it to the docker container rm command. For example, to remove the first two containers listed in the output above, you would run:

```
docker container rm cc3f2ff51cab cd20b396a061
```

If you get an error message similar to the one shown below, it means that the container is running. You’ll need to stop the container before removing it.

## Removing all stopped containers
```
docker container prune
```

If you want to get a list of all non-running (stopped) containers that will be removed with docker container prune, use the following command:

```
docker container ls -a --filter status=exited --filter status=created
```

## Removing containers using filters
certain condition using the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can specify more than one filter by using multiple --filter options.

For example, to remove all images created more than 12 hours ago, you would run:

```
docker container prune --filter "until=12h"
```

## Stop and remove all containers
To stop all running containers, enter the docker container stop command followed by the containers IDs:

```
docker container stop $(docker container ls -aq)
```
The command docker container ls -aq generates a list of all containers.
```
docker container rm $(docker container ls -aq)
```

## Removing Docker Images
When you download a Docker image, it is kept on the server until you manually remove it

### Removing one or more images
To remove one or more Docker images, first, you need to find the IDs of the images:

```
docker image ls
```

Once you’ve located the images you want to remove, pass their IMAGE ID to the docker image rm command. For example, to remove the first two images listed in the output above, you would run:
```
docker image rm 75835a67d134 2a4cca5ac898
```
If you get an error message like the one below, it means that an existing container uses the image. To remove the image, you will have to remove the container first.

Error response from daemon: conflict: unable to remove repository reference "centos" (must force) - container cd20b396a061 is using its referenced image 75835a67d134

### Removing dangling images
Docker provides a docker image prune command that can be used to remove dangled and unused images.

A dangling image is an image that is not tagged and is not used by any container. To remove dangling images, type:
```
docker image prune
```

### Removing all unused images
To remove all images that are not referenced by any existing container, not just the dangling ones, use the prune command with the -a option:
```
docker image prune -a
```

### Removing images using filters
With the docker image prune command, you can also remove images based on a particular condition with the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can use more than one filter.

For example, to remove all images that are created more than seven days (168 hours) ago, you would run:

```
docker image prune -a --filter "until=12h"
```

## Removing Docker Volumes

### Removing one or more volumes

To remove one or more Docker volumes, run the docker volume ls command to find the ID of the volumes you want to remove.
```
docker volume ls
```

Once you’ve found the VOLUME NAME of the volumes you want to remove, pass them to the docker volume rm command. For example, to remove the first volume listed in the output above, run:
```
docker volume rm 4e12af8913af888ba67243dec78419bf18adddc3c7a4b2345754b6db64293163
```
If you get an error similar to the one shown below, it means that an existing container uses the volume. To remove the volume, you will have to remove the container first.

Error response from daemon: remove 4e12af8913af888ba67243dec78419bf18adddc3c7a4b2345754b6db64293163: volume is in use - [c7188935a38a6c3f9f11297f8c98ce9996ef5ddad6e6187be62bad3001a66c8e]

### Removing all unused volumes
To remove all unused volumes, run the docker image prune command:
```
docker volume prune
```

## Removing Docker Networks
### Removing one or more networks
To remove one or more Docker networks, use the docker network ls command to find the ID of the networks you want to remove.
```
docker network ls
```

Once you’ve located the networks you want to remove, pass their NETWORK ID to the docker network rm command. For example, to remove the network with the name my-bridge-network, run:

```
docker network rm c520032c3d31
```

If you get an error similar to the one shown below, it means that an existing container uses the network. To remove the network, you have to remove the container first.

Error response from daemon: network my-bridge-network id 6f5293268bb91ad2498b38b0bca970083af87237784017be24ea208d2233c5aa has active endpoints


### Removing all unused network

Use the docker network prune command to remove all unused networks.
```
docker network prune
```

You’ll be prompted to continue:

WARNING! This will remove all networks not used by at least one container.
Are you sure you want to continue? [y/N] 

### Removing networks using filters
With the docker network prune command, you can remove networks based on condition using the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can use more than one filter by using multiple --filter options.

For example, to remove all networks that are created more than 12 hours ago, run:

```
docker network prune -a --filter "until=12h"  
```

## How to install additional software in the containers
```
docker-compose exec httpd /bin/bash -c "apt-get update && apt-get -y install nano"
```

```
docker-compose exec php /bin/bash -c "sudo yum -y install nano"  
```

## Can’t connect to local MySQL server through socket ‘/var/run/mysqld/mysqld.sock’ (2)” error in Docker ?
If you happen to get “Can’t connect to local MySQL server through socket ‘/var/run/mysqld/mysqld.sock’ (2)” errors in logging in to mysql-server while working on docker containers:

    check the path of socket file in /etc/mysql/my.cnf

    check if mysqld.sock and mysqld.pid file is present in /var/run/mysqld/ directory or not.

    If not, create these files as:
	
```
touch /var/run/mysqld/mysqld.sock
touch /var/run/mysqld/mysqld.pid
chown -R mysql:mysql /var/run/mysqld/mysqld.sock
chown -R mysql:mysql /var/run/mysqld/mysqld.sock
chmod -R 644 /var/run/mysqld/mysqld.sock
```

Now restart the mysql-server as:
```
service mysql-server restart
```
Now, trying logging again in to mysql-server.

## Docker compose 
### Lamp
```
https://github.com/sprintcube/docker-compose-lamp
```


## Versiones de docker
```
https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
```

## How to force Docker for a clean build of an image
There's a --no-cache option:


```
docker build --no-cache -t u12_core -f u12_core .

```

## Docker prune all
```
docker system prune --all --volumes --force
```

## How to reduce the size of RHEL/Centos/Fedora Docker image
Yes Docker image sizes can be dramatically reduced by doing a "yum clean all"

Initial RHEL Image Size = 196M

Dockerfile - RHEL Image(+bc) = 505M

```
# Build command
# docker build -t rhel7base:latest --build-arg REG_USER='<redhat_developer_user>' --build-arg REG_PSWD='<password>' --squash .

FROM registry.access.redhat.com/rhel7/rhel:latest

LABEL maintainer="tim"

ARG REG_USER=none
ARG REG_PSWD=none

RUN subscription-manager register --username $REG_USER --password $REG_PSWD --auto-attach && \
    subscription-manager repos --enable rhel-server-rhscl-7-rpms && \
    yum install -y bc
```

## Dockerfile - RHEL Image(+bc) with "yum clean all" = 207M saving 298M    
```
# Build command
# docker build -t rhel7base:latest --build-arg REG_USER='<redhat_developer_user>' --build-arg REG_PSWD='<password>' --squash .

FROM registry.access.redhat.com/rhel7/rhel:latest

LABEL maintainer="tim"

ARG REG_USER=none
ARG REG_PSWD=none

RUN subscription-manager register --username $REG_USER --password $REG_PSWD --auto-attach && \
    subscription-manager repos --enable rhel-server-rhscl-7-rpms && \
    yum install -y bc && \
    yum clean all && \
    rm -rf /var/cache/yum
```

# Cleaning local docker cache

## Context

Recently, my laptop was running out of space. I had the impression that docker would be the culprit and went to check its disk space usage, using the following command:

```
docker system df
```

It turns out that Docker was consuming close to 40% of the disk space available!

## Solution

Let’s go through the different options to free hard disk space.

## Removing unused containers

Docker containers have a status field indicating where they are at in their lifecycle. According to the docs, status can be one of created, restarting, running, removing, paused, exited, or dead.

First, we need to get the IDs of the containers with status exited or dead as follows:

```
docker ps --filter status=exited --filter status=dead -q
```

Then, we can reuse the above command to delete these containers with the following command:
```
docker rm $(docker ps --filter=status=exited --filter=status=dead -q)
```

A one-liner alternative to remove all stopped containers is:
```
docker container prune
```

## Removing all containers

First, we need to stop all running containers. We can get the IDs of the running containers as follows:

```
docker ps -q
```

Then, we can stop all the containers with:

```
docker stop $(docker ps -q)
```

You can replace docker stop with docker kill in the above command to forcibly stop the containers.

Finally, we can delete all containers:

```
docker rm $(docker ps -a -q)
```

## Removing dangling images

Dangling images, as mentioned by the documentation, are final images (i.e, not intermediary build layers) that no longer have an associated tag with them.

We can get the image ID for such images as follows:
```
docker images --filter dangling=true -q
```

Then, we can delete those images with the following command:
```
docker rmi $(docker images --filter dangling=true -q)
```
A one-liner alternative to remove all dangling images is:
```
docker image prune
```

## Removing all images

Docker doesn’t allow to remove images that have an associated container, so to really delete all images, it is necessary first to remove all containers.

Similarly to the previous section, we need the IDs of all the images, which we can get using:
```
docker images -a -q
```

Then, we can combine it with **docker rmi**:
```
docker rmi $(docker images -a -q)
```

A one-liner alternative to remove all images is:
```
docker image prune -a
```

## Removing volumes

Volumes also take space in the host machine. They are never deleted automatically since they may contain data that can be reused by different containers or directly from the host.

Then, to remove all docker volumes use:
```
docker volume prune
```

## Removing networks

Although docker networks don’t consume too much disk space, they create iptables rules, network devices and routing table entries. To prune these objects, you can run:

```
docker network prune
```

## Removing everything

Instead of manually pruning different types of resources, you may be interested on wiping out everything from your local cache. For that we can leverage the docker system prune command as follows:

To remove containers, images and networks use:
```
docker system prune
```

To remove containers, images, networks and volumes, use
```
docker system prune --volumes
```

## References
[Tutorial](https://renehernandez.io/snippets/cleaning-local-docker-cache/)


# Docker Exec into Container as Root
Docker is a powerful containerization tool that allows users to create isolated and standalone applications. Docker containers carry the base operating system, the applications, and all required packages. Hence, in some instances, we need to have access to the systems shell, execute commands and perform custom configurations. Luckily, Docker provides us with the functionality to run commands in running containers.

This tutorial aims to show you how to work with the Docker exec command to execute commands in running containers.

## Basic Usage

Working with Docker exec is very simple. We start by calling the docker exec command followed by the container name or id and the command to execute.

For example, to run the echo command in container Debian, we use the command as:
```
docker exec debian echo hello
```

The command spawns a shell of the Debian container and executes the echo command. An example output appears below:

To get the name or an ID of the running containers, use the command:

```
docker ps
```
## Docker Exec Options

Docker exec command supports various options to modify the functionality of the commands. It supports the following functions.

    -i – This option keeps the STDIN.
    -t – Spawns a pseudo TTY
    -u – Specifies the username or UID.
    -w – Working directory
    -p – allocates extended privileges to the command.
    -d – runs in detached mode.
    -e – sets environment variables.

## Docker Exec Sh

In most cases, we need a shell instance into the container to execute raw commands. To do this, we use the docker exec command.
```
docker exec debian -i -t /bin/bash
```

The command above launches an interactive shell. It is good to ensure bash executable exists before the running command.

If bash or any shell you wish to use is unavailable, use sh in the command below:
```
docker exec -it /bin/sh
```

As you can see, you have an interactive shell session where you can execute commands.

## Exec as Root

To exec command as root, use the -u option. The option requires a username or UID of the user. For example:
```
docker exec -u 0 debian whoami
```
In the above command, we use the UID of the root user to execute the whoami command as root.

To use the username instead of the user UID, use the command:
```
docker exec -u root debian whoami

```
The command above can help when you want to troubleshoot or perform tasks that require elevated privileges.

## Conclusion

That is all for the docker exec command.

We have discussed using docker exec to run commands in your running containers and spawn a shell session. Finally, we covered how to run commands as root using username and UID. 