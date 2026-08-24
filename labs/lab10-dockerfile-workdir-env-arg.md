# Lab 10 - Dockerfile WORKDIR, ENV and ARG

## Objective

Learn and practice WORKDIR, ENV, ARG, custom Dockerfile names, and Docker container troubleshooting.

## Step 1 - WORKDIR

Dockerfile:

    FROM ubuntu

    WORKDIR /app

    COPY index.html .

    CMD ["cat", "index.html"]

Build:

    docker build -t my-ubuntu:v3 .

Run:

    docker run --rm my-ubuntu:v3

Output:

    Hello from my Docker image

Verify the working directory:

    docker run --rm -it my-ubuntu:v3 bash

Inside the container:

    pwd

Output:

    /app

Check the file:

    ls -la
    cat index.html

The file exists at:

    /app/index.html

## Step 2 - ENV

Dockerfile:

    FROM ubuntu

    WORKDIR /app

    ENV APP_NAME="DockerLearning"
    ENV APP_ENV="development"

    COPY index.html .

    CMD ["cat", "index.html"]

Build:

    docker build -t my-ubuntu:v4 .

Run Bash inside the container:

    docker run --rm -it my-ubuntu:v4 bash

Check the environment variables:

    echo $APP_NAME
    echo $APP_ENV

Output:

    DockerLearning
    development

## Step 3 - ARG

Created a separate Dockerfile:

    Dockerfile.arg

Content:

    FROM ubuntu

    ARG APP_VERSION=1.0

    RUN echo "Building application version: $APP_VERSION"

    ENV APP_VERSION=$APP_VERSION

    CMD ["bash"]

## Step 4 - Build Using the Custom Dockerfile

Because the file is named Dockerfile.arg instead of Dockerfile, use -f:

    docker build -f Dockerfile.arg -t arg-demo:v1 .

The build output showed:

    Building application version: 1.0

The default ARG value was used.

## Step 5 - Override ARG

Build using a different value:

    docker build -f Dockerfile.arg --build-arg APP_VERSION=2.0 -t arg-demo:v2 .

The build output showed:

    Building application version: 2.0

This proved that --build-arg overrides the default ARG value.

## Step 6 - Verify ARG and ENV

Run:

    docker run --rm -it arg-demo:v2 bash

Inside:

    echo $APP_VERSION

After adding:

    ENV APP_VERSION=$APP_VERSION

and rebuilding the image, the value becomes available inside the running container.

Important observation:

ARG by itself is available during build time and is not automatically a runtime environment variable.

## Step 7 - Docker Run vs Docker Exec

Created and started a container using:

    docker run -it my-ubuntu:v3 bash

This created a new container and started Bash.

For an existing running container:

    docker exec -it <container-name> bash

runs Bash inside that existing container.

## Step 8 - Real-World Troubleshooting Lab

Scenario:

A developer reported that an Nginx web application was not accessible from the browser.

Initial container:

    docker run -d --name troubleshoot-web nginx

Check:

    docker ps

The container was running.

The PORTS column showed:

    80/tcp

Check:

    docker port troubleshoot-web

No host port mapping was returned.

Root cause:

The Nginx container was listening on port 80, but port 80 was not published to the host.

## Step 9 - Fix Port Mapping

Remove the old container:

    docker rm -f troubleshoot-web

Create it again with port mapping:

    docker run -d --name troubleshoot-web -p 8081:80 nginx

Verify:

    docker ps

Port mapping:

    0.0.0.0:8081->80/tcp

Check:

    docker port troubleshoot-web

Output:

    80/tcp -> 0.0.0.0:8081

The application became accessible through:

    http://localhost:8081

## Step 10 - Verify the Application

Check logs:

    docker logs troubleshoot-web

The logs showed:

    Configuration complete; ready for start up

This confirmed that Nginx started successfully.

Test from the host:

    curl http://localhost:8081

The request returned the Nginx welcome page.

The Nginx logs showed:

    GET / HTTP/1.1 200

This confirmed that the application was responding successfully.

## Troubleshooting Result

The original problem was caused by missing host-to-container port publishing.

The troubleshooting process was:

    Container running?
        ↓
    Check port mapping
        ↓
    No host mapping
        ↓
    Recreate container with -p
        ↓
    Verify with docker port
        ↓
    Test using curl
        ↓
    Application working

## Key Learning

A container being in the Running state does not automatically mean the application is accessible from the host.

Always collect evidence before deciding the root cause.