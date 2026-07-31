# Docker Image vs Docker Container

## What is a Docker Image?

A Docker Image is a read-only template that contains everything required to run an application, including:

- Application code
- Runtime
- Libraries
- Dependencies
- Configuration

A Docker image is used to create one or more containers.

---

## What is a Docker Container?

A Docker Container is a running instance of a Docker Image.

Containers are isolated environments where applications execute.

Unlike images, containers have a writable layer, allowing changes while they are running.

---

## Image vs Container

| Docker Image | Docker Container |
|--------------|------------------|
| Read-only template | Running instance of an image |
| Cannot execute by itself | Executes the application |
| Used to create containers | Created from an image |
| Immutable | Has a writable layer |

---

## Relationship

One Docker Image can create multiple Docker Containers.

Example:

Ubuntu Image
├── Container 1
├── Container 2
└── Container 3

---

## Example

Image:

ubuntu

Create a container:

docker run -it ubuntu bash

In this command:

- `ubuntu` is the image.
- Docker creates a new container from that image.
- `bash` starts a shell inside the container.

---

## Interview Questions

### What is the difference between an Image and a Container?

**Answer:**

A Docker Image is a read-only template that contains an application and all its dependencies. A Docker Container is a running instance of that image where the application executes.

---

## Summary

- Image = Blueprint
- Container = Running application
- One image can create multiple containers.