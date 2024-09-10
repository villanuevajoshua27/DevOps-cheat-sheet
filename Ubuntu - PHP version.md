# How to change PHP version and install packages

## Introduction
Step by step in how to change existing PHP version and install some packages

### Step 1
check the PHP version that is available and choose your desire
```
sudo update-alternatives --config php
```

### Step 2
here, I will assume to choose `PHP7.2` and install the package you want
```
sudo apt-get install php7.2-<package>
```

### Step 3
restart apache2 to apply changes
```
sudo systemctl restart apache2
```
