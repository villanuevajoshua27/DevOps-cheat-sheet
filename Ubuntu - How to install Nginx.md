## Installing Nginx on Ubuntu 
Nginx is a high-performance web server often used to serve websites, reverse proxy apps, or handle load balancing. This guide will help you install Nginx on an Ubuntu server step-by-step.

### ✅ Step 1: Update the system and install required packages
These packages allow your system to securely fetch files over HTTPS and manage software sources.

```bash
sudo apt update -y
sudo apt install -y curl gnupg2 ca-certificates lsb-release
 ``` 

### ✅ Step 2: Add the official Nginx repository and its signing key
This step ensures you're installing the latest stable version of Nginx directly from the official Nginx source.

```bash
curl -fsSL https://nginx.org/keys/nginx_signing.key | sudo gpg --dearmor -o /usr/share/keyrings/nginx-keyring.gpg
```

Now add the Nginx repository to your system’s list of software sources:

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/nginx-keyring.gpg] http://nginx.org/packages/ubuntu $(lsb_release -cs) nginx" | sudo tee /etc/apt/sources.list.d/nginx.list
```
 
### ✅ Step 3: Install Nginx
Once the repository is added, update your package list again and install Nginx:

```bash
sudo apt update
sudo apt install -y nginx
``` 

### ✅ Step 4: Start the Nginx service
Start the Nginx service so your server can begin handling HTTP traffic.

```bash
sudo systemctl start nginx
```

If you want Nginx to start automatically on system boot:

```bash
sudo systemctl enable nginx
 ```
 
### ✅ Step 5: Test if Nginx is working
Open your browser and visit: `http://your-server-ip`
 
> 📍 Replace your-server-ip with the public IP address of your server. If everything is working, you should see: `“Welcome to nginx!”`. This means the installation was successfu
