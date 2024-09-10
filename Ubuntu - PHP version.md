# How to change PHP version and install packages

## Introduction
Step by step in how to change existing PHP version and install some packages

### Step 1
Check the PHP version that is available and choose your desire
```bash
sudo update-alternatives --config php
```

### Step 2
Here, I will assume to choose `PHP7.2` and install the package you want
```bash
sudo apt-get install php7.2-<package>
```

### Step 3
Restart apache2 to apply changes
```bash
sudo systemctl restart apache2
```

### Step 4
Check your PHP version
```bash
php --version
```
