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
 
> 📍 Replace your-server-ip with the public IP address of your server. If everything is working, you should see: `“Welcome to nginx!”`. This means the installation was successful!
 
---

## ⚙️ Install PHP (With FPM and MySQL Support)
PHP is a widely-used scripting language for web development. This guide installs PHP, its FastCGI Process Manager (FPM), and MySQL extension so it can work with Nginx and MariaDB.
 
 
### ✅ Step 1: Install PHP with FPM and MySQL Extension
This command installs:
php-fpm: Needed to run PHP with Nginx

```bash
php-mysql: Allows PHP to talk to MySQL/MariaDB
```

php-cli: Enables running PHP scripts from the terminal

```bash
sudo apt install -y php-fpm php-mysql php-cli
```
 
### ✅ Step 2: Change Socket Owner to Nginx (Important for Compatibility)
Update the configuration so that Nginx has permission to access the PHP-FPM socket.

❗ IMPORTANT: Replace 8.4 with your actual PHP version. To check, run:

```bash
php -v
```

Run these two commands to change the owner and group:

```bash
sudo sed -i 's/listen.owner = www-data/listen.owner = nginx/g' /etc/php/8.4/fpm/pool.d/www.conf
sudo sed -i 's/listen.group = www-data/listen.group = nginx/g' /etc/php/8.4/fpm/pool.d/www.conf
```
 
After these steps, don't forget to restart PHP-FPM so changes take effect:

```bash
sudo systemctl restart php8.4-fpm
```

> ✅ Replace php8.4-fpm with your actual PHP version if it's different.

---

## 🌐 Creating Virtual Sites (Nginx Server Blocks) + SSL
This guide sets up a virtual host using Nginx, serves a test PHP page, and secures it with an SSL certificate using Certbot.
 
 
### ✅ Step 1: Create Document Root Folder
This is where your website files (e.g., HTML, PHP) will be stored.

Navigate to the default Nginx web root:

```bash
cd /usr/share/nginx/html/
```

Create a folder for your site (replace with your subdomain):

```bash
sudo mkdir /usr/share/nginx/html/example.com
```

### ✅ Step 2: Create a Test PHP File
This PHP file will help you test if PHP is working via the browser.

```bash
echo "<?php phpinfo(); ?>" | sudo tee /usr/share/nginx/html/example.com/index.php
```

### ✅ Step 3: Create a Virtual Host Config File
Go to the Nginx configuration folder:

```bash
cd /etc/nginx/conf.d/
```

Create a configuration file for your subdomain:

```bash
sudo nano /etc/nginx/conf.d/example.com.conf
```

Paste the following block inside the file:

```bash
server {
    listen 80;
    server_name example.com;
 
    root /usr/share/nginx/html/example.com;
    index index.php index.html index.htm;
 
    location / {
        try_files $uri $uri/ =404;
    }
 
    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass unix:/run/php/php8.4-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

⚠️ Make sure to:
Match the server_name to your domain.
Match the root to the folder you created.
Replace php8.4-fpm.sock with your actual PHP version if needed (php -v to check)

### ✅ Step 4: Add HTTPS with Certbot

Install Certbot and the Nginx plugin:
```bash
sudo apt install certbot python3-certbot-nginx -y
```

Run Certbot to get your SSL certificate:
```bash
sudo certbot --nginx
```

Follow the prompts and select your domain. 

### ✅ Step 5: Restart Services

After configuration, restart both Nginx and PHP to apply changes:
```bash
sudo systemctl restart nginx
sudo systemctl restart php8.4-fpm
```
> (Replace php8.4-fpm if your version is different.)
 
🛠 If You See Errors
Try restarting the correct PHP-FPM version:
```bash
sudo systemctl restart php8.3-fpm
```

If issues persist, open the config file and adjust socket permissions:

```bash
sudo nano /etc/php/8.3/fpm/pool.d/www.conf
```

🔁 Change the following lines back to www-data:

```bash
user = www-data
group = www-data
listen.owner = www-data
listen.group = www-data
```

Then restart PHP-FPM again:
```bash
sudo systemctl restart php8.3-fpm
```
 
## 🔐 GitHub Deploy Automation
This section shows you how to generate an SSH key pair and use it for secure automated deployment from GitHub.

#### ✅ Step 1: Generate an SSH Key Pair
This will create a private and public SSH key used to connect your server securely.

```bash
ssh-keygen -t ed25519 -C "github-deploy"
```

When prompted for a file to save the key, just press Enter to accept the default location (/home/ubuntu/.ssh/id_ed25519).
Optionally, leave the passphrase empty if this key will be used for automation.

This will generate two files:
/home/ubuntu/.ssh/id_ed25519 → your private key (keep this safe, never share!)
/home/ubuntu/.ssh/id_ed25519.pub → your public key (safe to share)
  
### ✅ Step 2: Add the Public Key to authorized_keys
This step tells your server to trust the public key when used to connect via SSH.

1. Open the file where trusted keys are stored:

```bash
sudo nano /home/ubuntu/.ssh/authorized_keys
```

2. Paste the contents of your public key file:


```bash
cat /home/ubuntu/.ssh/id_ed25519.pub
```

3. Copy the entire line and paste it into the authorized_keys file.
4. Save and exit: Press Ctrl + O, then Enter, then Ctrl + X.
 
✅ Now your server is ready to accept SSH connections from this key. You can use the private key (id_ed25519) in your GitHub Actions secrets for automated deployment.

---

## 🚀 GitHub Deployment SSH Setup 
This guide helps you set up SSH keys with GitHub and configure your server for deployment using GitHub Actions.
  
### ✅ Step 1: Add SSH Key to Your GitHub Account
This step allows your server to connect and pull code from GitHub securely.

Open your browser and go to:
🔗 https://github.com/settings/keys

Click the “New SSH Key” button.
Fill in the form:
- `Title`: Anything (e.g., My Server)
- `Key`: Paste the contents of your public key

Run this to get the key:

```bash
cat ~/.ssh/id_ed25519.pub
```

Click Add SSH Key.

### ✅ Step 2: Add Repository Secrets for Deployment
You’ll now add secure variables in GitHub so it can SSH into your server during deployment.
Open your browser and go to your repository’s settings:
🔗 https://github.com/your-username/your-repo/settings/secrets/actions

Click “New Repository Secret” three times to add the following:
- Name: `SSH_PRIVATE_KEY`
Secret: Paste the private key contents:
`cat ~/.ssh/id_ed25519`

```
(Starts with -----BEGIN OPENSSH PRIVATE KEY-----)
```

- Name: `SSH_USER`
Secret: `ubuntu`

- Name: `SSH_HOST`
Secret: your server’s IP address (e.g., 222.222.222.222)
  
### ✅ Step 3: Test SSH Connection from the Server
Use this command to verify that your SSH connection to GitHub is working:

```bash
ssh -T git@github.com
```

If successful, you should see a message like:
`Hi your-username! You've successfully authenticated.`

### ✅ Step 4: Modify NGINX Configuration (Optional Proxy Forwarding)
If your app uses a reverse proxy to serve PHP, Node.js, etc., you can update the NGINX config file.

1. Edit your domain’s NGINX config file:

```bash
sudo nano /etc/nginx/conf.d/example.com.conf
```

2. Replace or update the location / block with the correct proxy settings for your app or backend.
Example for Node.js reverse proxy:

```bash
location / {
    proxy_pass http://127.0.0.1:3000; # Adjust the port as needed
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

3. Save and exit.

### ✅ Step 5: Restart NGINX to Apply Changes

```bash
sudo service nginx restart
```

### ✅ Step 6: Install NPM and PM2

```bash
sudo apt install npm -y
sudo npm install -g pm2
```

---

## 🚀 Github Action Workflow
This guide explains how to automatically deploy your website or app to your LEMP server (Linux, Nginx, MariaDB, PHP/Node.js) whenever you push code to the main branch on GitHub.
 
### ✅ Step 1: Add Workflow File to Your Repository
Inside your GitHub project folder:

1. Create a new folder: `.github/workflows`
2. Create a file named `deploy.yaml`

Paste the following contents:

```yml
name: Deploy to LEMP
 
on:
  push:
    branches:
      - main
 
jobs:
  deploy:
    runs-on: ubuntu-latest
 
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
 
      - name: Set up SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          ssh-keyscan -H ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts
 
      - name: Deploy via SSH
        run: |
          ssh -o StrictHostKeyChecking=no -i ~/.ssh/id_ed25519 ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }} << 'EOF'
            set -e
 
            DEPLOY_DIR="/usr/share/nginx/html/example.com"
            TEMP_DIR="/tmp/deploy-tmp"
 
            echo "📥 Cloning repository..."
            rm -rf "$TEMP_DIR"
            git clone git@github.com:username/repository.git "$TEMP_DIR"
 
            echo "🧹 Cleaning and deploying to $DEPLOY_DIR..."
            sudo rm -rf "$DEPLOY_DIR"/*
            sudo cp -r "$TEMP_DIR"/* "$DEPLOY_DIR"
            sudo cp -r "$TEMP_DIR"/.[!.]* "$DEPLOY_DIR" || true  # copy hidden files
 
            cd "$DEPLOY_DIR"
 
            echo "📦 Installing dependencies..."
            sudo npm install
 
            echo "🚀 Building app..."
            sudo npm run build
 
            echo "🚀 Restarting app via PM2..."
            pm2 restart app-name-3000 || pm2 start npm --name "app-name-3000" -- start
 
            # Optional: Restart NGINX if needed
            # echo "🔄 Restarting Nginx..."
            # sudo systemctl restart nginx
          EOF
```
 
 
### 🔍 What This Does
1. Triggers on Push to main – Automatically starts when you push code to GitHub.
2. Connects to Your Server – Uses SSH to access your Ubuntu server.
3. Clones Your Repo – Downloads the latest version of your app.
4. Cleans and Copies Files – Clears the old deployment and replaces it with the new code.
5. Installs Dependencies – Runs npm install to install packages.
6. Builds the Project – Runs your build step (npm run build).
7. Restarts the App – Uses PM2 to restart the Node.js app (or starts it if it's the first time).
