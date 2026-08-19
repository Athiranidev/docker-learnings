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