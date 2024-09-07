## How to change PHP version and install packages
Step by step in how to change existing PHP version and install some packanges

- check the PHP version that are available and choose your desire
```
sudo update-alternatives --config php
```

- here, I will assume to choose `PHP7.2` and install package you want
```
sudo apt-get install php7.2-<package>
```

- restart apache2
```
sudo systemctl restart apache2
```
