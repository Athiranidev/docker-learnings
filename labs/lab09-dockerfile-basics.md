# Lab 09 - Dockerfile Basics

## Objective

Learn how to create a Dockerfile, build a custom Docker image, run a container from the image, and copy a file into an image.

## Step 1 - Create Lab Directory

    mkdir -p labs/dockerfile-demo
    cd labs/dockerfile-demo

## Step 2 - Create the First Dockerfile

    FROM ubuntu

    RUN apt-get update

    CMD ["bash"]

## Step 3 - Build the First Image

    docker build -t my-ubuntu:v1 .

Docker used Ubuntu as the base image and executed RUN apt-get update during the build.

The custom image was created as:

    my-ubuntu:v1

## Step 4 - Run the Custom Image

    docker run -it --name my-ubuntu-container my-ubuntu:v1

The container started Bash because the Dockerfile contained:

    CMD ["bash"]

Inside the container:

    cat /etc/os-release
    apt --version

Verified that the container was running Ubuntu and had apt available.

## Step 5 - Create a File on the Host

    echo "Hello from my Docker image" > index.html

Verify:

    cat index.html

Output:

    Hello from my Docker image

## Step 6 - Use COPY

Updated the Dockerfile:

    FROM ubuntu

    COPY index.html /app/index.html

    CMD ["cat", "/app/index.html"]

## Step 7 - Build Version 2

    docker build -t my-ubuntu:v2 .

Docker copied index.html into the image as:

    /app/index.html

The build output showed:

    COPY index.html /app/index.html

The image was created as:

    my-ubuntu:v2

## Step 8 - Run Version 2

    docker run --rm --name my-ubuntu-v2 my-ubuntu:v2

The CMD instruction runs:

    cat /app/index.html

Expected output:

    Hello from my Docker image

## Result

Successfully created custom Docker images:

    my-ubuntu:v1
    my-ubuntu:v2

Successfully learned and tested:

    FROM
    RUN
    COPY
    CMD
    docker build
    docker run
    docker start

## Key Learning

    Dockerfile
        ↓
    docker build
        ↓
    Docker Image
        ↓
    docker run
        ↓
    Container

RUN executes during image building, while CMD executes when the container starts.