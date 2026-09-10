# Docker Image Management

## What is Docker Image Management?

Docker image management is the process of building, tagging, storing, sharing, pulling, and removing Docker images.

Basic lifecycle:

Dockerfile
↓
docker build
↓
Docker Image
↓
docker tag
↓
Docker Registry
↓
docker push
↓
Docker Hub
↓
docker pull
↓
docker run
↓
Container

---

## Docker Image Tags

A Docker image is commonly referenced as:

<repository>:<tag>

Examples:

nginx:latest
nginx:1.29
my-nginx:v1
my-nginx:v2

The tag identifies a particular version or reference of an image.

If no tag is specified, Docker normally uses:

latest

Example:

docker pull nginx

is equivalent to:

docker pull nginx:latest

Versioned tags are useful for identifying and rolling back specific image versions.

---

## Docker Tag

`docker tag` creates another name/reference for an existing image.

Syntax:

docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Example:

docker tag my-nginx:v2 athiranidev/my-nginx:v1

This does not build a new image.

Both tags can point to the same image ID.

Example:

my-nginx:v2
        ↓
   Image ID
        ↑
athiranidev/my-nginx:v1

---

## Docker Registry

A Docker registry stores and distributes Docker images.

Docker Hub is a public Docker registry.

Example image name:

athiranidev/my-nginx:v1

Breakdown:

athiranidev → Docker Hub username
my-nginx    → repository
v1          → image tag

---

## Docker Login

`docker login` authenticates the Docker CLI with a container registry.

Example:

docker login -u athiranidev

Docker Hub can use a Personal Access Token (PAT) for CLI authentication.

Do not store or share the token in source code.

---

## Docker Push

`docker push` uploads a local image to a registry.

Example:

docker push athiranidev/my-nginx:v1

Flow:

Local Docker Image
↓
docker push
↓
Docker Hub Repository

---

## Docker Image Layers

Docker images consist of multiple layers.

When pushing an image, Docker checks whether layers already exist in the registry.

If a layer already exists, Docker can reuse/mount it instead of uploading it again.

Example output:

Mounted from library/nginx

This reduces unnecessary data transfer.

---

## Docker Pull

`docker pull` downloads an image from a registry to the local machine.

Example:

docker pull athiranidev/my-nginx:v1

Flow:

Docker Hub
↓
docker pull
↓
Local Docker Image

---

## Docker RMI

`docker rmi` removes an image reference from the local machine.

Example:

docker rmi athiranidev/my-nginx:v1

If another tag points to the same image, removing one tag does not necessarily remove the underlying image data.

---

## Docker Run from a Registry Image

An image pulled from Docker Hub can be directly used to create a container.

Example:

docker run -d --name pulled-nginx -p 8083:80 athiranidev/my-nginx:v1

Then test:

curl http://localhost:8083

---

## Complete Image Workflow

docker build
↓
docker tag
↓
docker login
↓
docker push
↓
Docker Hub
↓
docker pull
↓
docker run

This workflow is commonly used in CI/CD pipelines to build, store, and deploy container images.