
## Image and Container
Images are the blueprints for containers and it is created with layers. It is read-only so it can not be run but it can be built.

Containers are executable units of software. They are isolated (they have their own filesystem, data) and stateless(they can store data internally and it losts them without using volumes).

## Key Docker Commands


#### ⚡️ Creating
To build a Docker file and create an image
```
docker build .  
```

To create and start a container
```bash
docker run IMAGE_NAME_OR_ID
```

#### ⚡️ Naming and tagging
To assing a name and tag to your image
```
docker build -t NEW_IMAGE_NAME:NEW_IMAGE_TAG .  
```
To assing a name to the container
```
docker run --name NEW_CONTAINER_NAME  
```

#### ⚡️ Removing
To remove an image
```bash
 docker rmi IMAGE_ID
```

To remove an image forcefully (when you get an error as it is used by stopped container)
```bash
 docker rmi -f IMAGE_NAME_OR_ID
```

To remove all unused images 
```bash
 docker image prune -a
```

To remove a container. 
```bash
 docker rm CONTAINER_NAME_OR_ID
```

To remove a container automatically when it's stopped. 
```bash
 docker run --rm IMAGE_NAME_OR_ID
```




#### ⚡️ Interactive Mode
To want to use listening output and also entering input you can use interactive options.
```bash
  docker start -i CONTAINER_NAME_OR_ID
  docker run -i IMAGE_NAME_OR_ID
```
#### ⚡️Inspection
To inspect an image
```bash
  docker inspect IMAGE_NAME_OR_ID
```

#### ⚡️ Listing
For a full list of all commands 
```bash
docker --help
```

To list images
```bash
 docker images
```

To list just all running containers. 
```bash
 docker ps
```

To list all containers. 
```bash
 docker ps -a
```
#### ⚡️ Start and stop
To restart a container
```bash
 docker start CONTAINER_NAME_OR_ID
```

To stop a container. 
```bash
 docker stop CONTAINER_NAME_OR_ID
```

#### 🚀 start vs run commands
You can use both start and run commands to start a container but with a diffenrence. start command starts an existing container, but run command create and starts a new container.

#### ⚡️ Logging
To fetch the logs of a container. 
```bash
 docker logs CONTAINER_NAME_OR_ID
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

#Specify the running port but it is optional.
#You need to expose the port with -p when running the container.
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

For my Spring Boot example you can find commands below.
```
docker build -t hello-docker . 
docker run -p 8080:8080 hello-docker
```


#### 🚀 Image Readonly
Everything in the images is read only then, and you can't edit it by simply updating your code. Hence you need to rebuild the image to pick up the changes.

#### 🚀 Image Layer 
Every instruction in your file creates a layer and these are cached. So when you rebuild your image it will be builded faster.

## Pushing Image To Docker Hub

To pushe an image to the repository follow below steps.

- At Docker Hub create a respository.

- Clone your exist image with the same name as the repository you created.
```bash
docker tag EXIST_IMAGE_NAME DOCKER_HUB_REPO_NAME
```
- Login to Docker Hub
```bash
 docker login
```
- Push image to Docker Hub
```bash
 docker push  DOCKER_HUB_REPO_NAME
```
- Logout from Docker Hub
```bash
 docker logout
```
Below a step-by-step example from my spring boot hello world project.

```bash
 docker tag hello-docker aylincelik/spring-boot-hello-world
 docker login
 docker push aylincelik/spring-boot-hello-world
 docker logout
```

## Pulling Image From Docker Hub
You can pull an image from Docker Hub by below commands.
```bash
 docker pull IMAGE_NAME
```
or
```bash
 docker run IMAGE_NAME
```
But there is a important difference.
#### 🚀 pull vs run commands
Docker pull command always pull the latest version of the image. When you rebuild and push the image to the repo "pull" command will fetch the latest version of updated image.

Docker run command first checks locally if it can not find the image locally then fetch latest version of the image from Hub. But if it finds image locally then it uses the local version. When you update image from repo, this command will not fetch the latest version. 
