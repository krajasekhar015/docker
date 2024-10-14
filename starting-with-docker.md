Docker is an open-source platform that enables developers to automate the deployment, scaling, and management of applications using containerization. It allows you to package applications and their dependencies into containers, which are lightweight, portable, and consistent across different environments.

## Installation of Docker

We will install `docker` using the rpm repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**1. Set up the repository** 

Install the `yum-utils` package (which provides the `yum-config-manager` utility) and set up the repository.

We need sudo access to run the docker commands
```
sudo su -
```
Now, we can run the following commands to setup repository
```
yum install yum-utils -y 
```
```
yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```

**2. Install Docker Engine**

To install the latest version, run:
```
yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**start docker**
```
systemctl start docker
```

**Check status of docker**
```
systemctl status docker
```

