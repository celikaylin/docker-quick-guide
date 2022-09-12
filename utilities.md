
# Utilities

At this document I mention about some utilities topic.


## ENTRYPOINT vs CMD

CMD command in the Dockerfile overwrite all command after the image name on docker run.

In the below example `command_two` would be overwritten by `command_one` so `command_two` would not be run.

Dockerfile
```bash
FROM base_image
.
.
CMD [command_one]

```
Command line
```bash
docker run IMAGE_NAME command_two
```
ENTYRPOINT command is different at this point. The command that is after image name on docker run would be appended to ENTYRPOINT command.

In the below example `command_two` would be appended to `command_one` so `command_one + command_two` would be run.
Dockerfile
```bash
FROM base_image
.
.
ENTYRPOINT [command_one]

```
Command line
```bash
docker run IMAGE_NAME command_two
```


## .dockerignore File
With this document you can specify which files or folders should not be copied to the image whenever use the `COPY` instruction.

It can help you reduce the Docker image size and speedup for build command.

You can add into the `.dockerignore` file these ones

```bash
Dockerfile
.git
```

## Environment Variables
You can add env varibles in different ways.

- By using Dockerfile you should add `ENV` as below command into the Dockerfile.
```
FROM openjdk:11.0  

WORKDIR /app

COPY ./target/hello-docker-0.0.1-SNAPSHOT.jar /app

ENV PORT 8080

EXPOSE $PORT

CMD ["java", "-jar", "hello-docker-0.0.1-SNAPSHOT.jar"]
```

- By using command

Here is the Dockerfile example and you should use $ for env variables.

Command is here
```bash
docker run --env KEY=VALUE --env KEY2=VALUE2 IMAGE_NAME
```

```
FROM openjdk:11.0  

WORKDIR /app

COPY ./target/hello-docker-0.0.1-SNAPSHOT.jar /app

EXPOSE $PORT

CMD ["java", "-jar", "hello-docker-0.0.1-SNAPSHOT.jar"]
```

```bash
docker run --env PORT=8080 hello-docker
```

- By using `.env` file

You can create and add below key values into the .env file
```bash
KEY=VALUE
KEY2=VALUE2
```

```
FROM openjdk:11.0  

WORKDIR /app

COPY ./target/hello-docker-0.0.1-SNAPSHOT.jar /app

EXPOSE $PORT

CMD ["java", "-jar", "hello-docker-0.0.1-SNAPSHOT.jar"]
```

You can create and add below key values into the `.env` file
```bash
PORT=8080
```

