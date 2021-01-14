## Dockeer build image
docker build -t televida-activemq:5.9.0 .

## Rebuild container
docker-compose up -d --force-recreate --no-deps --build container

## Access bash container
docker exec -it transportdb bash

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