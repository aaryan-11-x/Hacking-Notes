# 1) Docker Pull
Before we can run a Docker container, we will first need an **image**. Images are *instructions for what a container should execute*.

```sh
docker pull nginx
```

By running this command, we are *downloading the latest version of the image titled “nginx”*. Images have these labels called _tags_. These _tags_ are used to refer to variations of an image. For example, an image can have the same name but different tags to indicate a **different version**.
When specifying a tag, you must include a colon `:` between the image name and tag, for example, `ubuntu:22.04` (image:tag).

| Docker Image | Tag    | Command Example                                          | Explanation                                                                                                                                                      |
| ------------ | ------ | -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ubuntu       | latest | docker pull ubuntu (Same as : docker pull ubuntu:latest) | This command will pull the latest version of the "ubuntu" image. If no tag is specified, Docker will assume you want the "latest" version if no tag is specified | 
| ubuntu       | 22.04  | docker pull ubuntu:22.04                                 | This command will pull version "22.04 (Jammy)" of the "ubuntu" image                                                                                             |


# 2) Docker Image ls
This command allows us to list all *images stored on the local system*. We can use this command to verify if an image has been downloaded correctly and to view a little bit more information about it (such as the tag, when the image was created and the size of the image).
![[Pasted image 20230601130511.png]]


# 3) Docker Image rm
If we want to remove an image from the system, we can use `docker image rm` along with the name (or Image ID).
```sh
docker image rm ubuntu:22.04

# Remove all stopped containers
docker rm $(docker ps -a -q -f status=exited)
OR
docker container prune

# Remove all containers
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```



# 4) Run a Docker Container
```sh
docker run -it helloworld /bin/bash
```

-   An image named "helloworld"
-   "Interactively" by providing the `-it` switch in the [OPTIONS] command. This will allow us to interact with the container directly.
-   I am going to spawn a shell within the container by providing `/bin/bash` as the [COMMAND] part. This argument is where you will place what commands you want to run within the container (such as a file, application or shell!)

| [Option] | Explanation                                                                                                                                                                                                                                                                                                              | Example                                                          |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------- |
| **-d**       | This argument tells the container to start in "detached" mode. This means that the container will run in the background.                                                                                                                                                                                                 | `docker run -d helloworld`                                         |
| **-it**      | This argument has two parts. The **"i"** means run interactively, and **"t"** tells Docker to run a shell within the container. We would use this option if we wish to interact with the container directly once it runs.                                                                                                        | `docker run -it helloworld`                                        |
| **-v**       | This argument is short for "Volume" and tells Docker to mount a directory or file from the host operating system to a location within the container. The location these files get stored is defined in the Dockerfile                                                                                                    | `docker run -v /host/os/directory:/container/directory helloworld` |
| **-p**       | This argument tells Docker to bind a port on the host operating system to a port that is being exposed in the container. You would use this instruction if you are running an application or service (such as a web server) in the container and wish to access the application/service by navigating to the IP address. | `docker run -p 80:80 webserver`                                    |
| **--rm**     | This argument tells Docker to remove the container once the container finishes running whatever it has been instructed to do.                                                                                                                                                                                            | `docker run --rm helloworld`                                     |
| **--name**   | This argument lets us give a friendly, memorable name to the container. When a container is run without this option, the name is two random words.                                                                                                                                                                       | `   docker run --name helloworld`                                |                                                                                                                                                                                                                                                                                                                        |                                                                  |

**Tip:** To list all containers (even stopped), you can use `docker ps -a`

# 5) Remove Docker volume
```sh
docker volume ls
docker volume rm volume_name volume_name
```

---
# Dockerfiles
Dockerfiles is a *formatted text file* which essentially *serves as an instruction manual* for what containers should do and ultimately assembles a Docker image.

| Instruction | Description                                                                                                                                             | Example                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| FROM        | This instruction sets a build stage for the container as well as setting the base image (operating system). ***All Dockerfiles must start with this***. | FROM ubuntu                                                                               |
| RUN         | This instruction will execute commands in the container within a new layer.                                                                             | RUN whoami                                                                                |
| COPY        | This instruction copies files from the local system to the working directory in the container (the syntax is similar to the `cp` command).              | COPY /home/cmnatic/myfolder/app/                                                          |
| WORKDIR     | This instruction sets the working directory of the container. (similar to using `cd` on Linux).                                                         | WORKDIR / (Sets to the root of file system)                                               |
| CMD         | This instruction determines what command is run when the container starts (you would use this to start a service or application).                       | CMD /bin/sh -c script.sh                                                                  |
| EXPOSE      | This instruction is used to tell the person who runs the container what port they should publish when running the container.                            | (tells the person running the container to publish to port 80 i.e. `docker run -p 80:80`) | 

#### Build Container
```sh
docker build -t helloworld .
```

- We are going to name it ourselves, so we are going to use the `-t` argument
- 1.  The Dockerfile is located in our current working directory (`.`)

```yml
# Use Ubuntu 22.04 as the base operating system of the container
FROM ubuntu:22.04

# Set the working directory to the root of the container
WORKDIR / 

# Create helloworld.txt
RUN touch helloworld.txt
```


#### Level Up Docker File

```yml
# THIS IS A COMMENT
FROM ubuntu:22.04

# Update the APT repository to ensure we get the latest version of apache2
RUN apt-get update -y 

# Install apache2
RUN apt-get install apache2 -y

# Tell the container to expose port 80 to allow us to connect to the web server
EXPOSE 80 

# Tell the container to run the apache2 service
CMD ["apache2ctl", "-D","FOREGROUND"]
```


##### Optimizing Docker File
```yml
FROM ubuntu:latest
RUN apt-get update -y && apt-get upgrade -y && apt-get install apache2 -y && apt-get install net-tools
```


