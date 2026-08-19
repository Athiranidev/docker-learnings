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

# Docker Networking

## What is Docker Networking?

Docker networking allows containers to communicate with each other, with the host machine, and with external networks.

Containers need networking when applications such as a backend, database, frontend, or web server need to communicate.

---

## Default Docker Networks

Docker provides three default networks:

| Network | Driver | Purpose |
|---|---|---|
| bridge | bridge | Default network for containers |
| host | host | Container shares the host's network stack |
| none | null | Container has no network connectivity |

List Docker networks:

```bash
docker network ls
```

---

## Bridge Network

The `bridge` network is Docker's default network.

When a container is started without specifying a network, Docker normally connects it to the default bridge network.

Inspect the bridge network:

```bash
docker network inspect bridge
```

Example configuration from the lab:

```text
Driver: bridge
Subnet: 172.17.0.0/16
Gateway: 172.17.0.1
```

---

## Subnet

A subnet is the IP address range available to containers connected to the network.

Example:

```text
172.17.0.0/16
```

Docker assigns container IP addresses from this network range.

---

## Gateway

The gateway is the entry and exit point for communication between the Docker network and other networks.

Example:

```text
Gateway: 172.17.0.1
```

Basic structure:

```text
Container
172.17.0.2
     |
     v
Gateway
172.17.0.1
     |
     v
Host / External Network
```

---

## Container IP Address

Docker automatically assigns an IP address to a container when it connects to a Docker network.

In our lab:

```text
Network:       bridge
Subnet:        172.17.0.0/16
Gateway:       172.17.0.1
Container:     network-demo
Container IP:  172.17.0.2
```

The container IP can be checked from inside the container:

```bash
hostname -I
```

Output:

```text
172.17.0.2
```

---

## Creating a Network Demo Container

Command:

```bash
docker run -dit --name network-demo ubuntu bash
```

### Meaning of `-dit`

`-dit` is a combination of three options:

```text
-d → Detached mode
-i → Keep STDIN open
-t → Allocate a terminal
```

### `-d`

Runs the container in the background.

### `-i`

Keeps the container's standard input open.

### `-t`

Allocates a terminal.

Therefore:

```bash
docker run -dit --name network-demo ubuntu bash
```

creates and starts an Ubuntu container with Bash while keeping it running in the background.

---

## Entering a Running Container

Use:

```bash
docker exec -it network-demo bash
```

This executes Bash inside the running container and provides an interactive terminal.

---

## Checking the Container IP

Inside the container:

```bash
hostname -I
```

Example output:

```text
172.17.0.2
```

This matched the IP address shown by:

```bash
docker network inspect bridge
```

---

## `ip addr` Command

We also tried:

```bash
ip addr
```

and received:

```text
bash: ip: command not found
```

This is not a Docker networking error.

The Ubuntu image used in the lab is minimal and does not include the `ip` command by default.

---

## Docker Network Inspection

The command:

```bash
docker network inspect bridge
```

provides detailed information about the network, including:

- Network name
- Network ID
- Network driver
- IPv4 configuration
- Subnet
- Gateway
- Connected containers
- Container IP addresses
- MAC addresses

---

## Port Mapping vs Container Networking

These are different concepts.

### Port Mapping

Example:

```bash
docker run -d -p 8080:80 nginx
```

This maps:

```text
Host port 8080
       |
       v
Container port 80
```

Port mapping allows external clients such as a browser on the host to access a service inside the container.

### Container Networking

Container networking allows containers to communicate through a Docker network.

Example:

```text
Container A
     |
     v
Docker Network
     |
     v
Container B
```

Container-to-container communication will be covered in the next networking lab.

---

## Key Learnings

- Docker provides networking functionality for containers.
- Docker has three default networks: `bridge`, `host`, and `none`.
- The default `bridge` network uses a subnet and gateway.
- Docker automatically assigns IP addresses to containers connected to a network.
- `docker network ls` lists Docker networks.
- `docker network inspect` shows detailed network information.
- `hostname -I` can be used inside a container to check its IP address.
- `docker exec -it` allows commands to be executed inside a running container.
- Port mapping and container networking are different concepts.

## User-Defined Bridge Network

A user-defined bridge network is a custom Docker network created by the user.

Create one with:

```bash
docker network create my-network 
```

Docker automatically creates a separate subnet and gateway for the network.

Example from the lab:

Network:  my-network
Driver:  bridge
Subnet:  172.18.0.0/16
Gateway: 172.18.0.1

Connect existing containers to the network:

docker network connect my-network demo-network
docker network connect my-network demo-network2

Inspect the network:

docker network inspect my-network

Containers connected to the network can communicate using their container names because Docker provides embedded DNS.

Example:

getent hosts demo-network2

Output:

172.18.0.3    demo-network2

This means Docker resolves the container name demo-network2 to its IP address.

A web server can also be accessed using its container name:

docker run -d --name web-server --network my-network nginx

Test communication using a temporary BusyBox container:

docker run --rm --network my-network busybox wget -qO- http://web-server

The Nginx welcome page was successfully returned, proving container-to-container communication through the user-defined network.

Key point: User-defined bridge networks provide container-name-based DNS resolution, making communication between multiple containers easier.

