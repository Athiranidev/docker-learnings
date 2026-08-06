# Docker

## What is Docker?

Docker is an open-source containerization platform that packages an application along with its dependencies, libraries, runtime, and configuration into a container. This ensures the application runs consistently across different environments.

---

## Why Docker?

Before Docker, developers often faced the problem:

> "It works on my machine."

This happened because development, testing, and production environments were different.

Docker solves this problem by packaging everything an application needs into a container.

---

## What does Docker package?

Docker packages:

- Application Code
- Runtime
- Libraries
- Dependencies
- Configuration Files

---

## Benefits of Docker

- Consistent environment
- Faster deployment
- Lightweight compared to Virtual Machines
- Easy to scale
- Portable across different systems

---

## Key Components

- Docker Engine
- Docker CLI
- Docker Hub
- Docker Image
- Docker Container

---

## Interview Definition

**Q: What is Docker?**

**Answer:**

Docker is an open-source containerization platform that packages an application along with all its dependencies into containers, ensuring it runs consistently across different environments.

---

## Summary

- Docker is a containerization platform.
- Containers share the host operating system kernel.
- Docker solves the "It works on my machine" problem.
- Docker makes applications portable and consistent.

## Docker Container Lifecycle

Docker containers move through different states during their lifecycle.

Created
↓
Running
↓
Stopped (Exited)
↓
Started Again
↓
Removed

Docker provides commands to manage each stage:

- docker run
- docker start
- docker stop
- docker restart
- docker exec
- docker rm

## Docker Images

- Docker image is a read-only template.
- One image can create multiple containers.
- Images contain application code, libraries and dependencies.

---

## Docker Image Layers

- Docker images are made up of multiple read-only layers.
- Layers are shared between images.
- Layers improve storage efficiency and download speed.

---

## Port Mapping

Syntax:

```bash
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

- Host Port = 8080
- Container Port = 80

---

## Docker Logs

Docker logs display the application's stdout and stderr.

Useful for:

- Startup messages
- Runtime logs
- Errors
- Troubleshooting

## Docker Image Removal

Docker images can be removed using the `docker rmi` command.

Syntax:

```bash
docker rmi <image_name>
```

Example:

```bash
docker rmi nginx
```

### Important Points

- `docker rm` removes containers.
- `docker rmi` removes images.
- An image cannot be removed if it is referenced by any existing container.
- The container can be in Running, Exited, or Created state.
- Remove the containers first, then remove the image.

### Verify Images

```bash
docker images
```

## Docker Volumes

A Docker volume is a Docker-managed persistent storage mechanism used to store data outside a container's writable layer.

### Key Points

- Data persists even if the container is removed.
- Volumes are managed by Docker.
- Multiple containers can mount the same volume.
- Volumes are stored on the host machine.
- By default, Docker stores volumes under:

```
/var/lib/docker/volumes/
```
## Docker Bind Mounts

A bind mount maps an existing file or directory from the host machine directly into a container.

### Key Points

- The host directory is chosen by the user.
- Docker does not manage the host directory.
- Changes made on the host are immediately reflected inside the container.
- Bind mounts are commonly used during application development.

### Docker Volume vs Bind Mount

| Docker Volume | Bind Mount |
|---------------|------------|
| Managed by Docker | Managed by the host |
| Docker chooses storage location | User chooses storage location |
| Best for persistent application data | Best for source code and development |
| Stored under `/var/lib/docker/volumes/` | Uses any existing host directory |