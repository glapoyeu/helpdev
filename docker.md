## pull image and latest version
docker pull mongoclient/mongoclient:4.0.0

## run it container as a daemon
docker run -d -p 3000:3000 mongoclient/mongoclient

## run container mongodb and persist data
docker run -d -p 3000:3000 -v C:\televida\local-environment\nosql\data_nosql:/data/db mongoclient/mongoclient

## Dockeer build image
docker build -t televida-activemq:5.9.0 .

## Rebuild container
docker-compose up -d --force-recreate --no-deps --build container

## Docker run new container
docker run --name televida-chat-gofitness -p 8085:80 -d televida-chat-web

## Access bash container
docker exec -it transportdb bash

## Access bash container and available tty
docker exec -it --tty chat_tomcat-api_1 bash

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
docker ps -a

## stopped containeres
docker stop <name/id>

## Start docker container exited
docker start -i <name/id>

## Remove images by id
docker image rm ae21c72df409

## Removing All Unused Docker Objects
docker system prune  

You’ll be prompt you to confirm the operation:
WARNING! This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all dangling images
        - all build cache
Are you sure you want to continue? [y/N]

If you want to remove all unused images not just the dangling ones, add the -a (--all) option to the command:

docker system prune -a

By default, the command doesn’t remove unused volumes to prevent losing important data. To remove all unused volumes, pass the --volumes option:
docker system prune --volumes

## Removing Docker Containers
Docker containers are not automatically removed when you stop them unless you start the container using the --rm flag.

### Removing one or more containers
To remove one or more Docker containers, use the docker container rm command, followed by the IDs of the containers you want to remove.

You can get a list of all containers by invoking the docker container ls command with the -a option:
docker container ls -a

Once you know the CONTAINER ID of the containers you want to delete, pass it to the docker container rm command. For example, to remove the first two containers listed in the output above, you would run:

docker container rm cc3f2ff51cab cd20b396a061

If you get an error message similar to the one shown below, it means that the container is running. You’ll need to stop the container before removing it.

## Removing all stopped containers
docker container prune

If you want to get a list of all non-running (stopped) containers that will be removed with docker container prune, use the following command:

docker container ls -a --filter status=exited --filter status=created

## Removing containers using filters
certain condition using the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can specify more than one filter by using multiple --filter options.

For example, to remove all images created more than 12 hours ago, you would run:

docker container prune --filter "until=12h"

## Stop and remove all containers
To stop all running containers, enter the docker container stop command followed by the containers IDs:

docker container stop $(docker container ls -aq)
The command docker container ls -aq generates a list of all containers.
docker container rm $(docker container ls -aq)

## Removing Docker Images
When you download a Docker image, it is kept on the server until you manually remove it

### Removing one or more images
To remove one or more Docker images, first, you need to find the IDs of the images:

docker image ls

Once you’ve located the images you want to remove, pass their IMAGE ID to the docker image rm command. For example, to remove the first two images listed in the output above, you would run:
docker image rm 75835a67d134 2a4cca5ac898
If you get an error message like the one below, it means that an existing container uses the image. To remove the image, you will have to remove the container first.

Error response from daemon: conflict: unable to remove repository reference "centos" (must force) - container cd20b396a061 is using its referenced image 75835a67d134

### Removing dangling images
Docker provides a docker image prune command that can be used to remove dangled and unused images.

A dangling image is an image that is not tagged and is not used by any container. To remove dangling images, type:

docker image prune

### Removing all unused images
To remove all images that are not referenced by any existing container, not just the dangling ones, use the prune command with the -a option:

docker image prune -a

### Removing images using filters
With the docker image prune command, you can also remove images based on a particular condition with the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can use more than one filter.

For example, to remove all images that are created more than seven days (168 hours) ago, you would run:

docker image prune -a --filter "until=12h"

## Removing Docker Volumes

### Removing one or more volumes

To remove one or more Docker volumes, run the docker volume ls command to find the ID of the volumes you want to remove.

docker volume ls

Once you’ve found the VOLUME NAME of the volumes you want to remove, pass them to the docker volume rm command. For example, to remove the first volume listed in the output above, run:

docker volume rm 4e12af8913af888ba67243dec78419bf18adddc3c7a4b2345754b6db64293163
If you get an error similar to the one shown below, it means that an existing container uses the volume. To remove the volume, you will have to remove the container first.

Error response from daemon: remove 4e12af8913af888ba67243dec78419bf18adddc3c7a4b2345754b6db64293163: volume is in use - [c7188935a38a6c3f9f11297f8c98ce9996ef5ddad6e6187be62bad3001a66c8e]

### Removing all unused volumes
To remove all unused volumes, run the docker image prune command:

docker volume prune

## Removing Docker Networks
### Removing one or more networks
To remove one or more Docker networks, use the docker network ls command to find the ID of the networks you want to remove.

docker network ls

Once you’ve located the networks you want to remove, pass their NETWORK ID to the docker network rm command. For example, to remove the network with the name my-bridge-network, run:

docker network rm c520032c3d31
If you get an error similar to the one shown below, it means that an existing container uses the network. To remove the network, you have to remove the container first.

Error response from daemon: network my-bridge-network id 6f5293268bb91ad2498b38b0bca970083af87237784017be24ea208d2233c5aa has active endpoints


### Removing all unused network

Use the docker network prune command to remove all unused networks.

docker network prune
You’ll be prompted to continue:

WARNING! This will remove all networks not used by at least one container.
Are you sure you want to continue? [y/N] 

### Removing networks using filters
With the docker network prune command, you can remove networks based on condition using the --filter option.

At the time of the writing of this article, the currently supported filters are until and label. You can use more than one filter by using multiple --filter options.

For example, to remove all networks that are created more than 12 hours ago, run:

docker network prune -a --filter "until=12h"
