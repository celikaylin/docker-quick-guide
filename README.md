
# Docker Quick Guide

This document includes quick information like some useful commands and tips.



![Docker](https://www.docker.com/wp-content/uploads/2022/03/horizontal-logo-monochromatic-white.png)


## Before Starting

 - [Docker Installation Document](https://docs.docker.com/get-docker/)
 - [Docker Hub](https://hub.docker.com/)
 - [Docker Playground](https://labs.play-with-docker.com/)
 - [VC Code](https://code.visualstudio.com/)
 - [Docker Extension For VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)
 
## Image and Container
Images are the blueprints.

Containers are executable units of software.


## Key Docker Commands

For a full list of all commands 
```bash
  docker --help
```

To create and start a container
```bash
  docker run IMAGE_NAME_OR_ID
```
Commands
``` 
docker run COMMAND_NAME IMAGE_NAME_OR_ID

  --name    : to assign a name to the container
  -d        : to run the container in detached mode
  -it       : to run the container in interactive mode
  --rm      : to remove container when it's stopped.

```

To create and start a container
```bash
  docker run IMAGE_NAME_OR_ID
```

To list just all running containers. 
```bash
 docker ps
```

To list all containers. 
```bash
 docker ps -a
```



## Dockerize

### Dokerfile

Dockerfiles are text documents that allow you to build images for Docker.

Here is the basic Docker file structure

```
# First step : Base image which is a name Docker will be recognized and will start by pulling it.
FROM base_image  

# To tell the Docker execute all commands in this folder
WORKDIR folder_path_and_name

#Second step : To tell which files on your local machines shoul go into the image.
COPY source destination

#run a command in the image to get some dependencies.. etc if needed for running app.
RUN some_command

#Specify the running port
EXPOSE port_number

#Then run your app
CMD run_app_command
```

### Spring Boot Docker File Example

Here is the Spring Boot dokerfile example

```
FROM openjdk:11.0  

WORKDIR /app

COPY ./target/hello-docker-0.0.1-SNAPSHOT.jar /app

EXPOSE 8080

CMD ["java", "-jar", "hello-docker-0.0.1-SNAPSHOT.jar"]
```
### Building And Running Container

To build Docker file
```
docker build .  
```
To assing a name to your image
```
docker build -t IMAGE_NAME .  
```
To run the container
```
docker run IMAGE_NAME  
```
To stop the container
```
docker stop IMAGE_NAME  
```

For my Spring Boot example you can find commands below.
```
docker build -t hello-docker . 
docker run -it 8080:8080 hello-docker
```

