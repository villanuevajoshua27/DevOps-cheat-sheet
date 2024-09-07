# Docker Commands
  
## Basic Docker Commands

### View Docker Version and Info

- `docker --version` check docker version
- `docker info` display system-wide information

### Images
- List of all images
```
docker images
```
- Pull an image from Docker Hub
```
# Skeleton
docker pull [image_name]:[tag]

# Example
docker pull nginx:alpine
```
- Build an image from a Dockerfile
```
# Skeleton
docker build -t [your_image_name]:[tag] .

# Example
docker build -t myapp:latest .
```
- Remove an image
```
# Skeleton
docker rmi [image_name]:[tag]

# Example
docker rmi nginx:alpine
```

### Containers

- List all running containers
```
docker ps
```
- Run a container (detached mode with port mapping)
```
# Skeleton
docker run -d -p [host_port]:[container_port] [image_name]:[tag]

# Example
docker run -d -p 8080:80 nginx:alpine
```
- Start a stopped container
```
docker start [container_id_or_name]
```
- Stop a running container
```
docker stop [container_id_or_name]
```
- Remove a stopped container
```
docker rm [container_id_or_name]
```
- Remove all stopped containers
```
docker container prune
```

### Interacting with Containers

- View the logs of a container
```
docker logs [container_id_or_name]
```
- Execute a command in a running container
```
docker exec -it [container_id_or_name] [command]
```
- Execute a command in a running container
```
docker exec -it [container_id_or_name] [command]

# Example (open a shell in the container):
docker exec -it [container_id_or_name] /bin/bash
```