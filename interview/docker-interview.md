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
