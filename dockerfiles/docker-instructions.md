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
* `CMD` instruction can be overridden at runtime

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

**Pushing our image to dockerhub**
* The format to push the image is `username/image-name:version`
* We need to login to dockerhub from the command prompt using `docker login -u username` and enter password
* Push the image using `docker push username/image-name:version`
* Pull the image using `docker pull username/image-name:version`

> Interview Question: RUN vs CMD <br>
`RUN` instruction runs at the time of image creation <br>
`CMD` instruction runs at the time of container creation

Whatever the command we give in `CMD` instruction it should run container for infinite times

**4. LABEL** : key-value pair
* The `LABEL` instruction adds metadata to an image (description, owner, project)
* `LABELS` are used to filter the images
* `LABEL` instruction won't create any functionality but only adds information to your image

```
LABEL key=value
```

**Example**
```
LABEL author="raja sekhar" \
      company="joindevops" \
      topic="dockerfiles"
```

**Filtering Docker Images**
```
docker images -f 'label=company=joindevops'
```

Using `docker inspect imageid` we can see the labels that we specified

> `MAINTAINER` instruction has been deprecated and `LABEL` instruction is much more flexible version of MAINTAINER instruction

**5. EXPOSE** : 
* The `EXPOSE` instruction informs Docker that the container listens on the specified network ports at runtime
* You can specify whether the port listens on TCP or UDP, and the default is TCP if you don't specify a protocol
* The EXPOSE instruction doesn't actually publish the port
* It functions as a type of documentation between the person who builds the image and the person who runs the container

```
EXPOSE portno/protocol
```

**Example**
```
EXPOSE 80
```

**6. ENV** : Sets environment variables in the image
* The `ENV` instruction sets the environment variable `<key>` to the value `<value>`
* These can be used inside container 

```
ENV key=value
```

**Example**
```
ENV course="devOps with AWS" \
	trainer="sivakumar" \
	duration="2hrs"
```
* When we build `docker build -t env:v1 .` and run `docker run env:v1` it won't run since we haven't given as instruction to run.
* If we use `docker ps -a`, there we can see that it has run default command "/bin/bash". It won't run container for infinite times. 
* So let us make it sleep for 100 seconds using `docker run -d env:v1 sleep 100` 
* Now, if we check the sleep command has been executed here and it will run for 100 seconds and get exited
* Within this 100 seconds, we can login to the container `docker exec -it containerid bash` and we can see environment variables which we have passed using `env` 

**7. COPY** : Copies files or directories from the host machine to the image
The `COPY` instruction copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>

```
COPY src dest
```

**Example**
```
COPY index.html /usr/share/nginx/html/index.html
```
Here, the index.html file should be in the place where dockerfile exists otherwise it can't access it

* Now, if we run `docker run -d -p 8080:80 copy:v1` and browse the IP address in browse, we get our index.html over there

**8. ADD** : Similar to `COPY
* The `COPY` instruction copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>
* It also supports extracting tar files and fetching files from URLs
```
ADD src dest
```

**Example**
```
ADD <url> /usr/share/nginx/html/index.html
```
Here, due to security reasons we can't access it since we have downloaded from internet. So, we need to give read access to all users 


**9. ENTRYPOINT** : Configures a container to run as an executable
* `ENTRYPOINT` instruction allows you to set default parameters for the container
* `ENTRYPOINT` instruction cannot be overridden like CMD `instruction`. If we try to override, then it will append to the `ENTRYPOINT` instruction
* For better results and best practices, we can use both of them together.
     * `CMD` instruction can provide arguments to `ENTRYPOINT` instruction. So, we can pass default arguments through `CMD` instruction and we can override them at run-time 

```
ENTRYPOINT ["executable","param"]
```
**Example**
```
CMD ["google.com"]
ENTRYPOINT ["ping"]
```
**Explanation**
```
ping google.com
```
It will send continuous packets until you stop it
```
ping -c 2 google.com
```
Here, `-c 2` specifies that you want to send 2 packets. In other words, it limits the number of pings to just two

**10. USER**
* The `USER` instruction sets the user name (or UID) and optionally the user group (or GID) to use as the default user and group for the remainder of the current stage
* The specified user is used for `RUN` instructions and at runtime, runs the relevant `ENTRYPOINT` and `CMD` commands
* For security you should not run containers using root user, it must be on normal user
> Container is a part of the host so it doesn't have seperate storage. 
* If we given root access to container it can access host and can get some secuirty issues
```
USER username
```
**Example**
```
USER expense
```

**11. WORKDIR** : Sets the working directory for subsequent instructions
* `WORKDIR` instruction is used to set the current working directory inside docker image
* `cd` command won't work in docker, so we are using `WORKDIR` instruction
```
WORKDIR /path/to/workdir
```
**Example**
```
WORKDIR /tmp/docker
```
**Command to find file in linu**
```
find / -iname "hello.txt"
```
Here, this command will find `hello.txt` file in linux server

**12. ARG**
* `ARG` instruction is used to set the variables at build time only but not inside the container
```
ARG key=value
```

**Example**
```
ARG course="devOps with AWS" \
	trainer="sivakumar" \
	duration="2hrs"
```
**Overridding default values of ARG**
```
docker build -t arg:v1 --build-arg course=docker
```
**ARG vs CMD**
* `ENV` variables can be accessed both in image build time and in container  
* `ARG` is only accessed at the time of image creation 
* You can use `ARG` instruction before `FROM` instruction in one special case i.e., to supply version to the bash image 
* `ARG` instruction before `FROM` instruction is only valid until FROM, it can't be accessed after FROM instruction

**How can I access ARG values inside container?** <br>
We can set arg value to env variable
```
ENV duration=$duration
```
**Interview Questions** <br>
RUN vs CMD  |  CMD vs ENTRYPOINT  |  ADD vs COPY  |  ARG vs ENV

**13. ONBUILD**
* The `ONBUILD` instruction adds to the image a trigger instruction to be executed at a later time, when the image is used as the base for another build
* It is used to trigger few instructions at build when a user is using our image
* When the image author builds, it won't trigger the `ONBUILD` instruction.
```
ONBUILD trigger-instruction
```
**Example**
```
ONBUILD COPY index.html /usr/share/nginx/html/index.html
```
**Explanation**
* Here, onbuild instruction runs only, when any user uses the image and it will throw an error saying that copy their own index.html to that location.
* To test this, create a folder with `index.html` file and `Dockerfile` and in `Dockerfile` use base image as our image 
