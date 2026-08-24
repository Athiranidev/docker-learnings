# Dockerfile

A Dockerfile is a text file containing instructions used to build a Docker image.

## Basic Flow

Dockerfile → docker build → Docker Image → docker run → Container

## Dockerfile Instructions

### FROM

Specifies the base image.

    FROM ubuntu

### RUN

Executes a command during the image build process.

    RUN apt-get update

RUN executes at build time and its changes become part of the image.

### COPY

Copies files from the Docker build context into the image.

    COPY index.html /app/index.html

Example:

    Host
      ↓
    index.html
      ↓
    Docker build
      ↓
    Image
      ↓
    /app/index.html

### CMD

Specifies the default command that runs when a container starts.

    CMD ["bash"]

CMD executes at container runtime, not during image building.

## RUN vs CMD

RUN → Build time
CMD → Container runtime

Example:

    RUN apt-get install -y nginx

Nginx is installed while building the image.

    CMD ["nginx", "-g", "daemon off;"]

Nginx starts when the container starts.

## Docker Build

    docker build -t my-ubuntu:v1 .

-t my-ubuntu:v1 → Image name and tag
. → Current directory used as the build context

## Docker Pull vs Docker Build vs Docker Run

docker pull → Downloads an existing image from a registry.

docker build → Creates an image using a Dockerfile.

docker run → Creates and starts a container from an image.

If the required image is not available locally, docker run can pull it from a registry.

## Build Context

The . in:

    docker build -t my-ubuntu:v2 .

means the current directory is the Docker build context.

Files inside the build context can be used by Dockerfile instructions such as COPY.

## Image Tags

    docker build -t my-ubuntu:v1 .
    docker build -t my-ubuntu:v2 .

v1 and v2 are tags used to identify image versions.

## Container Lifecycle

    docker run -it --name my-ubuntu-container my-ubuntu:v1

docker run creates and starts a new container.

    docker start my-ubuntu-container

docker start starts an existing stopped container.

exit stops a container but does not remove it.

## Practical Example

Dockerfile:

    FROM ubuntu

    COPY index.html /app/index.html

    CMD ["cat", "/app/index.html"]

The host file index.html is copied into the image as /app/index.html.

When the container starts, CMD executes:

    cat /app/index.html

and displays:

    Hello from my Docker image

## Key Learnings

- Dockerfile contains instructions for building an image.
- FROM defines the base image.
- RUN executes during image build.
- COPY copies files from the build context into the image.
- CMD defines the default runtime command.
- docker build creates an image.
- docker pull downloads an existing image.
- docker run creates and starts a container.
- docker start starts an existing stopped container.
- exit stops a container but does not remove it.

## WORKDIR

WORKDIR sets the working directory inside the image/container.

Example:

    WORKDIR /app

After setting WORKDIR to /app:

    COPY index.html .

copies the file to:

    /app/index.html

WORKDIR also creates the directory if it does not already exist.

Example:

    FROM ubuntu
    WORKDIR /app
    COPY index.html .
    CMD ["cat", "index.html"]

The container starts in /app, so CMD can use:

    cat index.html

## ENV

ENV defines environment variables that are available inside the running container.

Example:

    ENV APP_NAME="DockerLearning"
    ENV APP_ENV="development"

Inside the container:

    echo $APP_NAME
    echo $APP_ENV

Output:

    DockerLearning
    development

ENV is mainly used for application configuration such as:

    APP_ENV
    APP_NAME
    PORT
    API_URL

Do not hardcode sensitive secrets such as passwords or API keys in a Dockerfile.

## ARG

ARG defines a build-time variable.

Example:

    ARG APP_VERSION=1.0

The value can be changed during the build:

    docker build --build-arg APP_VERSION=2.0 -t my-app:v2 .

ARG is available during image building.

Example:

    ARG APP_VERSION=1.0
    RUN echo "Building application version: $APP_VERSION"

## ARG vs ENV

    ARG → Build time
    ENV → Container runtime

ARG is not automatically available as an environment variable inside the running container.

Example:

    ARG APP_VERSION=1.0

    RUN echo "Building version: $APP_VERSION"

If the value should also be available at runtime:

    ARG APP_VERSION=1.0
    ENV APP_VERSION=$APP_VERSION

Then:

    docker build --build-arg APP_VERSION=2.0 -t my-app:v2 .

The resulting container can access:

    echo $APP_VERSION

Output:

    2.0

Important:

Changing a Dockerfile does not change an already-built image. The image must be rebuilt for the changes to be included.

## Custom Dockerfile Name

Docker normally looks for a file named:

    Dockerfile

If another Dockerfile is used, specify it with -f:

    docker build -f Dockerfile.arg -t arg-demo:v1 .

Here:

    -f Dockerfile.arg → Use Dockerfile.arg
    -t arg-demo:v1    → Image name and tag
    .                 → Build context

## Docker Run vs Docker Exec

    docker run
    → Creates and starts a new container from an image.

    docker exec
    → Executes a command inside an existing running container.

Example:

    docker run -it my-ubuntu:v3 bash

Creates a new container and starts Bash.

Example:

    docker exec -it my-ubuntu-container bash

Runs Bash inside an existing running container.

Remember:

    docker run  → Image → New Container → Command
    docker exec → Running Container → Command

## -it

    -i → Interactive
    -t → Allocate a terminal

Together:

    -it

allows interactive work inside a container.

## --rm

    --rm

automatically removes the container after it exits.

Useful for temporary testing containers.

Example:

    docker run --rm -it my-ubuntu:v3 bash

## Port Publishing

The -p option publishes a container port to the host.

Example:

    docker run -d --name web-server -p 8080:80 nginx

Meaning:

    Host port 8080
          ↓
    Container port 80
          ↓
    Nginx

Syntax:

    -p HOST_PORT:CONTAINER_PORT

## EXPOSE vs -p

EXPOSE documents the port that the application expects to use inside the container.

Example:

    EXPOSE 80

EXPOSE does not publish the port to the host.

To make the application accessible from the host, use:

    -p 8080:80

Remember:

    EXPOSE → Documents container port
    -p     → Publishes/maps the port

## Docker Troubleshooting

When an application is not accessible, do not immediately restart the container.

Follow an evidence-based troubleshooting process:

    Symptom
       ↓
    Collect evidence
       ↓
    Narrow down the failing layer
       ↓
    Form a hypothesis
       ↓
    Test the hypothesis
       ↓
    Identify root cause
       ↓
    Fix
       ↓
    Verify

Typical checks:

    1. Is the container running?
    2. Check container logs.
    3. Is the application process running?
    4. Is the application listening on the expected port?
    5. Is the Docker port mapping correct?
    6. Can the application be reached from inside the container?
    7. Can the application be reached from the host?
    8. Check application configuration and environment variables.
    9. Check dependencies such as databases or other services.
    10. Verify the fix.

Useful commands:

    docker ps
    docker ps -a
    docker logs <container>
    docker inspect <container>
    docker port <container>
    docker exec -it <container> bash
    curl http://localhost:<port>

Important:

A running container does not necessarily mean the application inside it is healthy.

An open TCP port does not necessarily mean the web application itself is functioning correctly.

## Dockerized Nginx Application

A Dockerfile can be used to create a custom image from an existing base image.

For a web application, the Dockerfile can:

- Use a web-server image as the base image.
- Set the working directory.
- Define environment variables.
- Copy application files into the image.
- Document the application port.
- Use the startup command already provided by the base image.

### Nginx Web Root

The official Nginx image serves static files from:

    /usr/share/nginx/html

The default Nginx page can be replaced by copying a custom index.html into this directory.

### Base Image Commands

A base image may already contain instructions such as CMD or ENTRYPOINT.

When using such an image, we don't always need to redefine those instructions in our Dockerfile.

For example, the official Nginx image already provides the command required to start Nginx in the foreground.

### Host Port vs Container Port

The application listens on a port inside the container.

The host can expose that application using port mapping:

    -p HOST_PORT:CONTAINER_PORT

Example:

    -p 8081:80

Here:

    8081 → Host port
    80   → Nginx container port

The host port can be different from the application's container port.

### Application Verification

A browser can be used to test a web application, but curl can also be used to test the HTTP service directly.

This helps determine whether a problem is with the application/Docker or with the browser.

Example:

    curl http://localhost:8081


## ENTRYPOINT

ENTRYPOINT defines the main executable of a container.

Example:

    ENTRYPOINT ["echo"]

The ENTRYPOINT is used when the container starts.

## CMD

CMD provides the default command or default arguments.

Example:

    CMD ["Hello Docker"]

## ENTRYPOINT + CMD

ENTRYPOINT and CMD can work together.

Example:

    FROM ubuntu

    ENTRYPOINT ["echo"]

    CMD ["Hello Docker"]

Running:

    docker run --rm entrypoint-demo:v1

effectively executes:

    echo "Hello Docker"

If an argument is provided during docker run:

    docker run --rm entrypoint-demo:v1 "Hello DevOps"

the CMD value is replaced.

The effective command becomes:

    echo "Hello DevOps"

The ENTRYPOINT remains unchanged.

## CMD Only vs ENTRYPOINT + CMD

CMD only:

    CMD ["echo", "Hello Docker"]

A runtime command can replace the entire CMD.

ENTRYPOINT + CMD:

    ENTRYPOINT ["echo"]
    CMD ["Hello Docker"]

ENTRYPOINT acts as the main executable and CMD provides default arguments.

## --entrypoint

The --entrypoint option can override the ENTRYPOINT defined in the image.

Example:

    docker run --rm --entrypoint printf entrypoint-demo:v2 "Hello %s\n" DevOps

This effectively runs:

    printf "Hello %s\n" DevOps

Output:

    Hello DevOps

This can also be useful for troubleshooting an image by temporarily replacing
its normal entrypoint with a shell or another command.

## Container Exit Status

A container remains running only while its main process is running.

If the main process finishes, the container stops.

Example:

    Exited (0)

means the main process completed successfully.

Exited (0) does not necessarily mean there was an error.

If the main process exits immediately, the container will also exit.

## Image Name vs Container Name

Example:

    docker run --rm entrypoint-demo:v1

entrypoint-demo:v1 → Image name

If --name is not specified, Docker automatically generates a container name.

Example:

    jovial_golick → Container name

To specify a container name:

    docker run --name my-container entrypoint-demo:v1

## --rm

    --rm

automatically removes the container after it exits.

Useful for temporary testing containers.