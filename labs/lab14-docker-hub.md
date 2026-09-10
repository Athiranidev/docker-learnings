# Lab 14 — Docker Hub Image Push and Pull

## Objective

Practice pushing a Docker image to Docker Hub and pulling it back to the local machine.

The lab demonstrates the basic Docker image distribution workflow:

Build → Tag → Login → Push → Pull → Run

---

## Prerequisites

- Docker installed
- Docker Hub account
- Docker Hub Personal Access Token
- Existing Docker image

---

## Step 1 — Check Local Images

Command:

docker images

Existing image used:

my-nginx:v2

---

## Step 2 — Tag the Image

Create a Docker Hub-compatible tag:

docker tag my-nginx:v2 athiranidev/my-nginx:v1

Verify:

docker images

Both tags should point to the same image ID.

Example:

my-nginx:v2
athiranidev/my-nginx:v1

---

## Step 3 — Login to Docker Hub

Command:

docker login -u athiranidev

Authenticate using a Docker Hub Personal Access Token.

Expected result:

Login Succeeded

---

## Step 4 — Push Image to Docker Hub

Command:

docker push athiranidev/my-nginx:v1

The image layers are uploaded to the Docker Hub repository.

Some base image layers may show:

Mounted from library/nginx

This means the registry already contains those layers and they can be reused.

---

## Step 5 — Remove the Local Docker Hub Tag

To simulate downloading the image again:

docker rmi athiranidev/my-nginx:v1

Verify:

docker images

The Docker Hub tag is removed locally.

---

## Step 6 — Pull the Image

Command:

docker pull athiranidev/my-nginx:v1

Expected result:

Downloaded newer image for athiranidev/my-nginx:v1

Verify:

docker images

The image should appear again with the same image ID.

---

## Step 7 — Run the Pulled Image

Command:

docker run -d --name pulled-nginx -p 8083:80 athiranidev/my-nginx:v1

Verify:

docker ps

Expected port mapping:

8083 → 80

---

## Step 8 — Test the Application

Command:

curl http://localhost:8083

Expected output:

Hello from my Dockerized application

This confirms that the image pulled from Docker Hub contains the application and can be used to create a working container.

---

## Step 9 — Clean Up

Remove the test container:

docker rm -f pulled-nginx

Verify:

docker ps

No running containers should remain.

---

## What I Learned

- `docker tag` creates another reference to an existing image.
- Tags do not necessarily represent separate image data.
- Docker Hub acts as a remote image registry.
- `docker push` uploads images to a registry.
- `docker pull` downloads images from a registry.
- Docker can reuse existing image layers.
- An image pulled from Docker Hub can be directly used with `docker run`.
- `docker rmi` can remove an image tag while other tags may still reference the same image.
- Docker Hub is an important part of the container image delivery workflow.

---

## Image Lifecycle

Dockerfile
↓
Build
↓
Local Image
↓
Tag
↓
Docker Hub
↓
Push
↓
Pull
↓
Run
↓
Container