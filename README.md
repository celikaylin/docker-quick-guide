
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

