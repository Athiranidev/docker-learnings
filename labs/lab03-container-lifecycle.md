# Lab 03 - Docker Container Lifecycle

## Objective

Learn the complete lifecycle of a Docker container using practical commands.

---

## Environment

- OS: Ubuntu 24.04 LTS
- Docker Engine
- Image: ubuntu

---

## Lab Steps

### 1. Create a New Container

```bash
docker run -dit --name ubuntu-demo ubuntu bash
```

**Result:**
- Created a new Ubuntu container.
- Container started in detached mode.

---

### 2. Verify Running Containers

```bash
docker ps
```

**Observation:**
- The `ubuntu-demo` container is in the `Up` state.

---

### 3. Enter the Running Container

```bash
docker exec -it ubuntu-demo bash
```

Inside the container:

```bash
mkdir docker-lab
cd docker-lab
echo "Learning Docker" > notes.txt
cat notes.txt
pwd
```

**Observation:**
- Successfully entered the running container.
- Created a directory and a text file.

---

### 4. Exit the Container

```bash
exit
```

**Observation:**
- Exited the shell.
- Container continued running because only the shell process exited.

---

### 5. Stop the Container

```bash
docker stop ubuntu-demo
```

**Observation:**
- Container state changed from `Up` to `Exited`.

---

### 6. View All Containers

```bash
docker ps -a
```

**Observation:**
- `ubuntu-demo` is displayed with status `Exited`.

---

### 7. Start the Existing Container

```bash
docker start ubuntu-demo
```

**Observation:**
- Existing container started successfully.

---

### 8. Restart the Container

```bash
docker restart ubuntu-demo
```

**Observation:**
- Docker stopped and started the same container.

---

### 9. Remove the Container

```bash
docker stop ubuntu-demo
docker rm ubuntu-demo
```

**Observation:**
- Container was removed successfully.

---

## Commands Learned

```bash
docker run
docker ps
docker ps -a
docker exec
docker stop
docker start
docker restart
docker rm
```

---

## Key Learnings

- `docker run` creates and starts a new container.
- `docker start` starts an existing stopped container.
- `docker stop` gracefully stops a running container.
- `docker restart` restarts the same container.
- `docker exec` opens a new process inside a running container.
- `docker ps` lists only running containers.
- `docker ps -a` lists all containers.
- `docker rm` removes a stopped container.

---

## Interview Takeaways

### Difference between `docker run` and `docker start`

- `docker run` creates a new container from an image.
- `docker start` starts an existing stopped container.

### Difference between `docker exec` and `docker run`

- `docker run` creates a new container.
- `docker exec` executes a command inside an already running container.

### Difference between `docker stop` and `docker rm`

- `docker stop` stops the container.
- `docker rm` removes the container.