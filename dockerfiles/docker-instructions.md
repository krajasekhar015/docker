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



**2. RUN** : Executes commands in the image during the build process
* **COPY** : Copies files or directories from the host machine to the image
* **ADD** : Similar to `COPY`, but also supports extracting tar files and fetching files from URLs
* **CMD** : Specifies the default command to run when a container is started from the image. Only one CMD instruction is allowed in a Dockerfile
* **ENTRYPOINT** : Configures a container to run as an executable. It allows you to set default parameters for the container
* **ENV** : Sets environment variables in the image
* **EXPOSE** : Documents the port that the container listens on at runtime. This does not actually publish the port; it’s mainly for documentation purposes
* **WORKDIR** : Sets the working directory for subsequent instructions