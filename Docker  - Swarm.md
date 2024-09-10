# How to install Docker Swarm on Ubuntu 22.04

## Introduction
Docker Swarm is a native clustering and orchestration tool for Docker containers. It allows you to create and manage a cluster of Docker hosts, making it easy to deploy and scale containerized applications. This tutorial will guide you through the process of installing Docker Swarm on Ubuntu 22.04.

---

## Prerequisites
Before you begin, ensure you have:
  1. An Ubuntu 22.04 server or desktop system
  2. SSH access to the server (optional)
  3. Root or sudo privileges

### Step 1: Install Docker

Update the package repository and install Docker
```
sudo apt update
sudo apt install -y docker.io
```

Start and enable the Docker service:
```
sudo systemctl start docker
sudo systemctl enable docker
```

### Step 2: Initialize Docker Swarm
Initialize Docker Swarm on the manager node:
```
sudo docker swarm init
```
> *Make note of the token displayed in the output. You will need this token to join worker nodes in the swarm.*

### Step 3: Join Worker Nodes
To join worker nodes to the swarm, run the following command on each worker node:
```
sudo docker swarm join --token [your_token] [manager_ip]:2377
```
> *Replace [your_token] with the token obtained from the manager node and [manager_ip] with the IP address of the manager node.*

### Step 4: Verify Swarm Status
Verify the status of the Docker Swarm:
```
sudo docker node ls
```
> *You should see the manager and worker nodes listed.*

### Conclusion
Congratulations! You have successfully installed Docker Swarm on Ubuntu 22.04. You can now use Docker Swarm to manage and orchestrate containers across multiple nodes in your cluster.
> reference [here](https://netcloud24.com/index.php?rp=/knowledgebase/65/-How-to-Install-Docker-Swarm-on-Ubuntu-22.04.html)

---

## Essential Docker swarm commands

### Status commands
- Check docker swarm status `sudo docker info`

### Nodes commands
- Check Swarm Nodes `sudo docker node ls`
- Remove Node from Swarm `sudo docker swarm leave --force`

### Network commands
- List all networks `sudo docker network ls`
- Add a network using Swarm `sudo docker network create --driver overlay <network_name>`
- Remove an existing network `sudo docker network rm <network_name>`

### Stack and Services commands

- Deploy your stack
  ```bash
  sudo docker stack deploy -c <yourfile.yml> <stack_name>
  
  # Sample
  sudo docker stack deploy -c compose.yml prod
  ```

- Remove an existing stack deployment
  ```bash
  sudo docker stack rm <stack_name>
  
  # Sample
  sudo docker stack rm prod
  ```

### Stack Services commands
- Check all services (for all stacks)
  ```
  sudo docker service ls

  # OR

  sudo docker stack ls
  ```
  
- Check services for a specific stack
  ```bash
  sudo docker stack services <stack_name>
  
  # Sample
  sudo docker stack services prod
  ```

- Restart a specific service in the stack
  ```bash
  sudo docker service update --force <service_name>
  
  # Sample
  sudo docker service update --force prod_api
  ```



---

## How to build and run independently

- Sample Query
  ```bash
  sudo docker build -t danegigi/gs2-api:local ./api
  sudo docker run -d -p 9000:9000 --name gs2-api danegigi/gs2-api:local
  ```
