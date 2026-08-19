# Docker Commands

## Check Docker Version

```bash
docker --version
```

**Purpose:**
Displays the installed Docker version.

---

## Run Hello World Container

```bash
docker run hello-world
```

**Purpose:**
Downloads the `hello-world` image (if not available locally), creates a container, runs it, displays a success message, and exits.

---

## Run an Ubuntu Container

```bash
docker run -it ubuntu bash
```

**Purpose:**
Creates a new Ubuntu container and starts an interactive Bash shell.

### Options

- `-i` → Interactive mode (keeps STDIN open)
- `-t` → Allocates a terminal (TTY)
- `ubuntu` → Docker image
- `bash` → Command executed inside the container

---

## List Running Containers

```bash
docker ps
```

**Purpose:**
Displays all currently running containers.

---

## List All Containers

```bash
docker ps -a
```

**Purpose:**
Displays all containers, including running and stopped ones.

---

## Start an Existing Container

```bash
docker start -ai <container_id>
```

**Purpose:**
Starts a stopped container and attaches your terminal to it.

### Options

- `-a` → Attach to the container
- `-i` → Interactive mode

---

## List Docker Images

```bash
docker images
```

**Purpose:**
Displays all Docker images available on the local machine.

---

## Notes

- `docker run` creates a **new container**.
- `docker start` starts an **existing stopped container**.
- `docker ps` shows **running containers only**.
- `docker ps -a` shows **all containers**.

# Docker Container Lifecycle Commands

## Create a New Container
docker run -it ubuntu bash

## Create and Run in Background
docker run -dit --name ubuntu-demo ubuntu bash

## List Running Containers
docker ps

## List All Containers
docker ps -a

## Enter a Running Container
docker exec -it ubuntu-demo bash

## Stop a Container
docker stop ubuntu-demo

## Start a Stopped Container
docker start ubuntu-demo

## Start and Attach
docker start -ai ubuntu-demo

## Restart a Container
docker restart ubuntu-demo

## Remove a Container
docker rm ubuntu-demo

## Force Remove a Running Container
docker rm -f ubuntu-demo

# Docker Image Commands

## List Images

```bash
docker images
docker image ls
```

## Download an Image

```bash
docker pull nginx
```

## Run Nginx Container

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

## View Container Logs

```bash
docker logs my-nginx
```

## Follow Logs

```bash
docker logs -f my-nginx
```

# Docker Image Removal Commands

## List Images

```bash
docker images
docker image ls
```

## Remove an Image

```bash
docker rmi nginx
```

## Remove an Image using Image ID

```bash
docker rmi <image_id>
```

## Docker Volume Commands

```bash
docker volume create my-volume
docker volume ls
docker volume inspect my-volume
docker volume rm my-volume
```
# Docker Bind Mount Commands

## Run a Container with a Bind Mount

```bash
docker run -d \
--name nginx-bind \
-p 8082:80 \
-v ~/DevOps/docker/docker-learnings/bind-demo:/usr/share/nginx/html \
nginx
```

## Enter the Running Container

```bash
docker exec -it nginx-bind bash
```

## Verify Mounted Files

```bash
cat /usr/share/nginx/html/index.html
```

---

# Docker Networking Commands

## List Networks

```bash
docker network ls
```

Lists all Docker networks.

## Inspect a Network

```bash
docker network inspect bridge
```

Displays detailed information about a Docker network.

## Run a Container in Background Mode

```bash
docker run -dit --name network-demo ubuntu bash
```

Runs an interactive container in detached mode.

## Enter a Running Container

```bash
docker exec -it network-demo bash
```

Opens an interactive Bash shell inside the running container.

## Check Container IP

Inside the container:

```bash
hostname -I
```

Displays the container's IP address.

## Dockerfile Commands

docker build -t <image>:<tag> .

docker run <image>:<tag>

docker start <container>

docker images

docker pull <image>