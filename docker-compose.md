# Docker Compose

Docker compose makes multi containers's setups easier.

To just build images not starting services
```bash
 docker-compose build
```

To build, pull images and start the containers in the *.yaml* file (To start services)
```bash
 docker-compose up
```

To start the services in the *docker-compose.yaml* file with detached mode
```bash
 docker-compose up -d
```

To force building images before starting the services.
```bash
 docker-compose up --build
```

To shut the services down (it both removes created networking, containers)
```bash
 docker-compose down
```

To shut the services down (it also removes volumes)
```bash
 docker-compose down -v
```

## Docker Compose File Structere

Here is detail of the structure
```bash
#compose specification version. You can check versions from https://docs.docker.com/compose/compose-file/compose-versioning/
version: 'specific_version'

# Service needs at least one child element
# Every child element needs a name and it is up to you.
# Every child element must be at the same level.
services:
  child-element-one:
    # You can give the name of the builded image
    image: 'image_name'

    # Or you can give the path of the Dockerfile and the image will be builded when it ups.
    build: docker_file_relative_path

    ports:
      -'host_port:container_port'

    volumes:
      - volumeName:containerPath

    # To force giving a name to the container
    container_name: container_name

    #You can give env varibales with name and value
    environment:
      - name1=value1
      - name2=value2
    #Or you can create a file (like file_name.env) and add the varibales in it then give the path here
    env_file:
      - relative_path_of_env_file

    # Docker will create a default network and puts all these containers in it. 
    # So you dont need to create a network when you want to put them in the same network.
    networks:
      - networkName

    #To tell the app works with other containers.  
    depends_on:
      - serviceName
  child-element-two:

  child-element-three:


# this is needed if you want to use named volumes. 
volumes:
  volumeName1: 
  volumeName2: 
```

Here is the my example. It has one database, one api and the api gets data from database. So I need two containers setup as below.


```bash
version: '3.8'

services:
  movie-backend:
      build: ./movies
      ports:
        - '3000:8000'
      networks:
        - movie-net
      volumes:
        - logs:/data/logs
        - ./movies:data  # This is bind mounts
      environment:
        - MONGODB_USERNAME=aylin
        - MONGODB_PASSWORD=****
      depends_on:
        - movie-db
  movie-db:
      image: 'mongo'
      volumes:
        - mongo-vol:/data/db
      environment:
        - MONGO_INITDB_ROOT_USERNAME=aylin
        - MONGO_INITDB_ROOT_PASSWORD=****
      networks:
        - movie-net
  volumes:
    data: 
    logs:
```