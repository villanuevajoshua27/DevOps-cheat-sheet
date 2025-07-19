# How to install Docker and Docker-Compose server

### Direct installation
Copy and paste this command for the full installation of Docker and Docker Compose Server.

```bash

#!/bin/bash
# Update package lists and upgrade installed packages
sudo apt-get update -y
sudo apt-get upgrade -y

# Install necessary prerequisites
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable repository for Docker
sudo echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package lists again
sudo apt-get update -y

# Install Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '(?<=\"tag_name\": \")[^\"]*')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Install zip and unzip (optional)
sudo apt install zip | sudo apt install unzip

# Install awscli (optional)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install ohmyzsh (optional)
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" -y

# Add permission to ubuntu user (optional)
sudo usermod -aG sudo ubuntu | sudo usermod -aG docker ubuntu

# Verify installations and log output
sudo docker --version
sudo docker-compose --version
zsh --version
groups ubuntu
whoami

```

### Background installation
Use this command if you want to run the installation in the background, or paste this in cloud-init or user-data.

```bash
#!/bin/bash

# Run in the background
(
# Update package lists and upgrade installed packages
sudo apt-get update -y
sudo apt-get upgrade -y

# Install necessary prerequisites
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common

# Add Docker's official GPG key
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Set up the stable repository for Docker
sudo echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package lists again
sudo apt-get update -y

# Install Docker
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '(?<=\"tag_name\": \")[^\"]*')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Install zip and unzip (optional)
sudo apt install zip | sudo apt install unzip

# Install awscli (optional)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install ohmyzsh (optional)
sudo apt install zsh -y
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" -y

# Add permission to ubuntu user (optional)
sudo usermod -aG sudo ubuntu | sudo usermod -aG docker ubuntu

# Verify installations and log output
sudo docker --version
sudo docker-compose --version
zsh --version
groups ubuntu
whoami

) > /var/log/user-data-install.log 2>&1 &  # End the background process and log output
```
