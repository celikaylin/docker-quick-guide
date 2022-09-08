
# Containers and Networking


## Connection Between a Container and WWW

Containers can send request to the World Wide Web (WWW). Hence you dont need any configuration or setup while connecting WWW .

## Connection Between a Container and Host Machine

To understand this topic firstly I got the issue and then solved it.

#### 🚀 Understanding and getting the issue
When you want to make connection from your dockerized application to your localhost by using *localhost* domain you will get connection refused error.

For handling this issue I have created two simple Rest APIs. One of them ([films](https://github.com/celikaylin/docker-networking/tree/master/films)) will work at the container and the other one([fav-films](https://github.com/celikaylin/docker-networking/tree/master/fav-films)) will work at my host machine. Then the dockerized project will connect to the fav-films api. (You can find my basic spring boot applications source code at [here](https://github.com/celikaylin/docker-networking)

#### 🚀 Solving the problem
At this problem if you want to connect your host machine correctly you can use *host.docker.internal* domain. Docker can recognize this special domain and then it's translated to the your IP address.

To solve problem follow these steps.
- Change your dockerized application configuration as below 
```bash
 localhost or 127.0.0.0 --> host.docker.internal
```
In my example I changed resource file as below.
```bash
 external-service.favUrl=http://localhost:8100/api              (Before)
 external-service.favUrl=http://host.docker.internal:8100/api   (After)
```
- Make sure you clean and build your project (for spring boot mvn clean install).
- Create a new image 
```bash
 docker build -t films .
```
- Create and run a container
```bash
 docker run --name films-container -p 3000:3000 -d --rm films
```
- Browse the url then it works...
```bash
 http://localhost:3000/api/favFilms
```

## Connection Between Different Containers

The containers which are the part of the same network can talk to each other.

The key point is how can we manage the IP addresses. For that you can use container name as the domain. The domain will be translated to the regarding container IP address.

To connect two containers at the same network follow below steps. 

- Create a specific network
```bash
 docker network create films-net
```

- Dockerized first app (fav-films) in the created network
```bash
docker build -t fav-films .
docker run --name fav-films-container --network films-net -d --rm -p 8100:8100  fav-films
```

- Change the domain of second project(films) which will connect to the first dockerized app(fav-films) as below.
```bash
external-service.favUrl=http://localhost:8100/api           (Before)
external-service.favUrl=http://fav-films-container:8100/api (After)
```
- Clean and build the project

- Dockerized the second app(films) in the created network
```bash
docker build -t films .
docker run --name films-container --network films-net -d --rm -p 3000:3000  films
```

- Browse it then it works
```bash
http://localhost:3000/api/films
http://localhost:3000/api/favFilms
```
