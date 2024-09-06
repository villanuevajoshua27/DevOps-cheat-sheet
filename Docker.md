# All about Docker

<details>
<summary>Docker commands</summary>
  
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

</details>


<details>
<summary>Steps to Create an EC2 Instance with Docker and docker-compose Installed on Ubuntu</summary>
<br/>
  
- Upload or paste this syntax in `user data` in advance settings.
```

#!/bin/bash
# Update package lists and upgrade installed packages
apt-get update -y
apt-get upgrade -y

# Install necessary prerequisites
apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable repository for Docker
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package lists again
apt-get update -y

# Install Docker
apt-get install -y docker-ce docker-ce-cli containerd.io

# Start and enable Docker
systemctl start docker
systemctl enable docker

# Install Docker Compose
curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '(?<="tag_name": ")[^"]*')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose

# Verify installations
docker --version
docker-compose --version

```

</details>

<details>
<summary>How to push and pull Docker registry</summary>
  
### Push the image
1. Ensure you have already created a repository in your Docker Hub.
2. Sign in to Docker Hub using the `docker login -u <USERNAME>` command.
3. You'll need to tag your local images with the correct repository and tag them before pushing them to Docker Hub using `docker tag`.
```
# skeleton
docker tag <image>:<tag> <USERNAME>/<REPOSITORY_NAME>:<NEW-IMAGE-TAG>

# sample
docker tag nginx:alpine <javillanueva/repository:nginx-alpine
```

4. Push the images to Docker Hub
```
# skeleton
docker push <USERNAME>/<REPOSITORY_NAME>:<NEW-IMAGE-TAG>

# sample
docker push javillanueva/repository:nginx-alpine
```

### Run the image on a new instance

> reference [here](https://docs.docker.com/get-started/workshop/04_sharing_app/)
</details>

