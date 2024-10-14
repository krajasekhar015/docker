Docker is an open-source platform that enables developers to automate the deployment, scaling, and management of applications using containerization. It allows you to package applications and their dependencies into containers, which are lightweight, portable, and consistent across different environments.

## Installation of Docker

We will install `docker` using the rpm repository

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**1. Set up the repository** <br>
Install the `yum-utils` package (which provides the `yum-config-manager` utility) and set up the repository.
```
sudo yum install yum-utils -y 
```
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
```