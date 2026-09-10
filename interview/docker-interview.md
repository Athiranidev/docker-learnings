# Docker Interview Questions

## 1. What is Docker?

**Answer:**

Docker is an open-source containerization platform that packages an application along with its dependencies into containers, ensuring it runs consistently across different environments.

---

## 2. Why do we use Docker?

**Answer:**

Docker solves the "It works on my machine" problem by providing a consistent runtime environment from development to production.

---

## 3. What is Containerization?

**Answer:**

Containerization is the process of packaging an application along with its dependencies into an isolated container so it can run consistently on different systems.

---

## 4. What is a Docker Image?

**Answer:**

A Docker Image is a read-only template containing an application, its runtime, libraries, dependencies, and configuration.

---

## 5. What is a Docker Container?

**Answer:**

A Docker Container is a running instance of a Docker Image.

---

## 6. Difference between Image and Container?

| Image | Container |
|--------|-----------|
| Blueprint | Running instance |
| Read-only | Writable layer |
| Used to create containers | Executes the application |

---

## 7. Docker vs Virtual Machine?

| Docker | Virtual Machine |
|---------|-----------------|
| Shares host kernel | Has its own kernel |
| Lightweight | Heavy |
| Starts in seconds | Takes longer to boot |
| Uses fewer resources | Uses more resources |

---

## 8. Explain the command:

```bash
docker run -it ubuntu bash
docker → Docker CLI
run → Creates and starts a new container
-i → Interactive mode
-t → Allocates a terminal
ubuntu → Image name
bash → Command executed inside the container

## Docker Container Lifecycle Interview Questions

### Difference between docker run and docker start

docker run creates a new container from an image.

docker start starts an existing stopped container.

---

### Difference between docker exec and docker run

docker run creates a new container.

docker exec runs a command or opens a shell inside an existing running container.

---

### What happens when you exit after docker exec?

Only the shell process started by docker exec exits.

The container keeps running because its main process (PID 1) is still active.

---

### Difference between docker ps and docker ps -a

docker ps shows only running containers.

docker ps -a shows all containers including stopped ones.

---

### Difference between docker stop and docker rm

docker stop stops a running container.

docker rm removes a stopped container.

## Docker Image Interview Questions

### What is a Docker Image?

A Docker image is a read-only template used to create containers.

---

### Difference between docker pull and docker run

docker pull downloads only the image.

docker run downloads (if required), creates and starts a container.

---

### What does -p 8080:80 mean?

8080 is the host port.

80 is the container port.

Docker forwards traffic from host port 8080 to container port 80.

---

### Difference between docker logs and docker exec

docker logs displays application logs.

docker exec executes commands inside a running container.

---

### Can two containers use the same host port?

No.

Each host port can be bound to only one container.

Different host ports such as 8080 and 8081 should be used.

## Docker Image Removal Interview Questions

### Difference between docker rm and docker rmi

- `docker rm` removes containers.
- `docker rmi` removes images.

---

### Why can't Docker remove an image?

Docker does not allow an image to be removed if it is referenced by one or more containers.

The containers must be removed first.

---

### Does stopping a container allow the image to be removed?

No.

Even a stopped (Exited) or Created container still references its image.

---

### Does removing a container automatically remove the image?

No.

The image remains in the local Docker image cache until `docker rmi` is executed.

## Docker Volumes Interview Questions

### What is a Docker Volume?

A Docker volume is persistent storage managed by Docker that stores data independently of containers.

---

### Why are Docker volumes used?

Docker volumes preserve application data even after containers are removed.

---

### Where are Docker volumes stored?

By default:

```
/var/lib/docker/volumes/
```

---

### What does `docker volume inspect` do?

Displays detailed information about a Docker volume, including its mount point, driver, and metadata.

---

### Does deleting a container delete its volume?

No. Volumes exist independently and must be removed explicitly.

## Docker Bind Mount Interview Questions

### What is a Bind Mount?

A bind mount maps an existing directory or file from the host machine directly into a container.

---

### Difference between Docker Volume and Bind Mount?

Docker volumes are managed by Docker and are mainly used for persistent application data.

Bind mounts use an existing host directory and are mainly used during development to share source code and configuration files.

---

### When should you use Bind Mounts?

Use bind mounts when developing applications and you want changes made on the host to be immediately visible inside the container.

---

# Docker Networking Interview Questions

## What is Docker Networking?

Docker networking provides communication between containers, the host machine, and external networks.

## What are the default Docker networks?

Docker provides three default networks:

- `bridge`
- `host`
- `none`

## What is the bridge network?

The bridge network is Docker's default network for containers that are not explicitly connected to another network.

## What is a subnet?

A subnet is an IP address range used by a network.

Example:

```text
172.17.0.0/16
```

## What is a gateway?

A gateway is the entry and exit point between a network and other networks.

Example:

```text
172.17.0.1
```

## How does a Docker container get an IP address?

Docker's network system automatically assigns an IP address to a container when it connects to a network.

## How can you check a Docker network?

Use:

```bash
docker network inspect <network-name>
```

## How can you check a container's IP from inside the container?

Use:

```bash
hostname -I
```

## What is the difference between port mapping and Docker networking?

Port mapping allows traffic from the host or external clients to reach a container service, while Docker networking provides communication between containers and networks.

## What does `docker exec -it` do?

It allows a command to be executed interactively inside a running container.

Example:

```bash
docker exec -it network-demo bash
```

## Dockerfile Interview Questions

### What is a Dockerfile?

A Dockerfile is a text file containing instructions used to build a Docker image.

### What is FROM?

FROM specifies the base image used to build the Docker image.

### What is RUN?

RUN executes commands during the image build process.

### What is CMD?

CMD specifies the default command that runs when a container starts.

### What is COPY?

COPY copies files from the build context into the Docker image.

### What is the difference between RUN and CMD?

RUN executes during image build, while CMD executes when the container starts.

### What is the difference between docker build and docker run?

docker build creates an image from a Dockerfile, while docker run creates and starts a container from an image.

### What is the difference between docker pull and docker build?

docker pull downloads an existing image from a registry, while docker build creates a new image using a Dockerfile.

### What does the . mean in docker build?

The . specifies the current directory as the Docker build context.

### What is a Docker build context?

The build context is the set of files available to Docker during an image build.

### What happens if a container already exists with the same name?

docker run creates a new container, so Docker returns a name conflict. Use docker start to start the existing stopped container, or remove the old container and create a new one.

### Does exit remove a container?

No. exit stops the container but does not remove it.

### Why is CMD ["nginx", "-g", "daemon off;"] commonly used?

It keeps Nginx running in the foreground so it remains the main process of the container.

### Can a Dockerfile contain multiple RUN instructions?

Yes. Each RUN instruction creates a new image layer.

### Can a Dockerfile have multiple CMD instructions?

A Dockerfile should have only one effective CMD. If multiple CMD instructions are specified, only the last one takes effect.


## Docker Compose

### What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML file (`compose.yaml`).

### Why use Docker Compose?

It allows us to define and manage multiple services, networks, volumes, ports, environment variables, and dependencies in one configuration file instead of running multiple `docker run` commands separately.

### Important Concepts

- **Service** → A definition of a container/application component in `compose.yaml`.
- **Service name** → Used as a hostname for service-to-service communication through Docker DNS.
- **Network** → Compose automatically creates a project-specific default network.
- **Bind mount** → Maps a specific host file/directory into a container; commonly used for development.
- **Docker volume** → Docker-managed persistent storage; commonly used for application/database data.
- **depends_on** → Controls service startup order.
- **healthcheck** → Checks whether a container/application passes a defined health test.
- **service_healthy** → Allows a dependent service to wait until the dependency passes its healthcheck.

### Important Commands

```bash
docker compose config
docker compose up -d
docker compose ps
docker compose ps -a
docker compose logs
docker compose stop
docker compose start
docker compose restart
docker compose down
docker compose run --rm <service> <command>

## Docker Image Management & Docker Hub

### 1. What is a Docker image tag?

A tag is a reference used to identify a particular version or variant of a Docker image.

Example:

docker:latest
myapp:v1
myapp:v2

---

### 2. What does `docker tag` do?

`docker tag` creates another name/reference for an existing Docker image.

Example:

docker tag my-nginx:v2 athiranidev/my-nginx:v1

It does not build a new image.

---

### 3. What is a Docker registry?

A Docker registry is a system used to store and distribute Docker images.

Examples include Docker Hub and private container registries.

---

### 4. What is Docker Hub?

Docker Hub is a hosted container registry where Docker images can be stored and distributed.

Example:

athiranidev/my-nginx:v1

---

### 5. What does `docker push` do?

`docker push` uploads a local Docker image to a container registry.

Example:

docker push athiranidev/my-nginx:v1

---

### 6. What does `docker pull` do?

`docker pull` downloads an image from a container registry to the local Docker environment.

Example:

docker pull athiranidev/my-nginx:v1

---

### 7. Why does Docker show "Mounted from library/nginx" during a push?

Docker images are made of layers.

If a layer already exists in the registry, Docker can reuse that layer instead of uploading it again.

This reduces data transfer and speeds up image pushes.

---

### 8. What is the difference between `docker push` and `docker pull`?

docker push:

Local → Registry

docker pull:

Registry → Local

---

### 9. What is the complete Docker image delivery workflow?

docker build
↓
docker tag
↓
docker login
↓
docker push
↓
Docker Registry
↓
docker pull
↓
docker run

---

### 10. Why use versioned image tags instead of only `latest`?

Versioned tags such as:

myapp:v1
myapp:v2
myapp:v3

make image versions easier to identify and support predictable deployments and rollbacks.

`latest` can point to different image contents over time.

---

### 11. Does `docker tag` create a new image?

No.

It creates another reference to an existing image.

Two tags can point to the same image ID.

---

### 12. Can a Docker image pulled from Docker Hub be run directly?

Yes.

Example:

docker run -d --name my-container -p 8080:80 athiranidev/my-nginx:v1

The image is used to create and start a container.