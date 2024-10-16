# Docker File & Docker Instructions

## Docker File
A Dockerfile is a text document that contains a series of docker instructions for building a custom Docker image.

`Dockerfile is a declarative way of creating custom images using docker instructions`

## Docker Instructions
Docker instructions are commands used in a Dockerfile to define how a Docker image should be built.

Here, the file name should be `Dockerfile`

**1. FROM** : represents base OS

* The `FROM` instruction initializes a new build stage and sets the base image for subsequent instructions.
* `FROM` should be the first instruction in the dockerfile

```
FROM base-OS:version
```

**Example**

```
FROM almalinux:9
```
* Here, almalinux == RHEL
* If we don't specify version, then by default it will take latest version

> When we build this instruction, docker will pull almalinux from dockerhub

*Command to Build:* `docker build -t image-name:version .`

**2. RUN** : Configure Image 

* The `RUN` instruction will execute any commands (like installing packages) to create a new layer on top of the current image
* `RUN` instruction runs at the time of image building

```
RUN command
```

**Example**

```
RUN dnf install nginx -y 
```

**3. CMD** : Specifies the default command to run when a container is started from the image

* `CMD` instruction sets the command to be executed when running a container from an image
* It runs at the time of container creation
* It keeps container running
* We can specify the CMD instruction using shell or exec forms
* There can only be one `CMD` instruction in a Dockerfile. If you list more than one `CMD`, only the last one takes effect

**Explanation**
* For example, in VM's to start backend, we use `systemctl start backend` command. Usually it run for infinite time if there is no problem in server
* So, there will be command which keeps the service running for infinite times

> `systemctl` only works for full fledged servers. It will not work for containers. 

We need to run manual commands to start service in containers.

For example, manual command to start nginx service `nginx -g daemon off` 

Usually, systemctl runs in background but manual commands runs in foreground

```
CMD ["executable","param1","param2"]
```
This is executable format

**Example**
```
CMD ["nginx", "-g", "daemon off;"]
```
**NOTE:**
* `docker build` represents image creation. Here `RUN` instruction will execute. 
    * *command:* `docker build -t image-name:version .` 
* `docker run` represents container creation --> Here `CMD` instruction will execute 
    * *command:* `docker run image-name:version` This command will run in foreground
    * *command:* `docker run -d image-name:version` If we specify `-d` flag then it will run in background 

> We need to re-build image everytime when the code gets changed





* **COPY** : Copies files or directories from the host machine to the image
* **ADD** : Similar to `COPY`, but also supports extracting tar files and fetching files from URLs
* 
* **ENTRYPOINT** : Configures a container to run as an executable. It allows you to set default parameters for the container
* **ENV** : Sets environment variables in the image
* **EXPOSE** : Documents the port that the container listens on at runtime. This does not actually publish the port; it’s mainly for documentation purposes
* **WORKDIR** : Sets the working directory for subsequent instructions