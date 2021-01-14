## Rebuild container
docker-compose up -d --force-recreate --no-deps --build container

## Access bash container
docker exec -it transportdb bash

## Access bash container with permission root user
docker exec -it --user root transportapp bash