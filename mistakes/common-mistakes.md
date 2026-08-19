# Common Mistakes

## Mistake 1: Used `ps -a` instead of `docker ps -a`

### Wrong

```bash
ps -a
```

### Correct

```bash
docker ps -a
```

### Lesson Learned

- `ps` displays Linux processes.
- `docker ps` displays Docker containers.

---

## Mistake 2: Confused `docker run` with `docker start`

### Lesson Learned

- `docker run` creates a **new container** and starts it.
- `docker start` starts an **existing stopped container**.

---

## Mistake 3: Expected files created inside a container to appear on the host

### Lesson Learned

- Containers have their own filesystem.
- Files created inside a container remain inside that container unless you use volumes or copy them to the host.

## Common Mistakes

### Mistake 1

❌ docker run -ai ubuntu-demo

Reason:
docker run expects an image, not a container name.

Correct:
docker start -ai ubuntu-demo

---

### Mistake 2

❌ docker exec -t ubuntu-demo abc

Reason:
abc is not a valid executable.

Correct:
docker exec -it ubuntu-demo bash

---

### Mistake 3

Thinking docker stop removes the container.

Reality:
docker stop only stops the container.

docker rm removes it.

## Common Mistakes

### Mistake

Trying to use the same host port for multiple containers.

Incorrect:

```bash
docker run -d -p 8080:80 nginx
docker run -d -p 8080:80 nginx
```

Correct:

```bash
docker run -d -p 8080:80 nginx
docker run -d -p 8081:80 nginx
```

---

### Mistake

Confusing docker logs with docker exec.

docker logs is used for viewing application logs.

docker exec is used for executing commands inside a running container.

## Common Mistakes

### Mistake

Trying to remove an image while containers still exist.

Incorrect:

```bash
docker rmi nginx
```

Error:

```
conflict: unable to delete image
```

Correct:

```bash
docker rm my-nginx nginx1 nginx2
docker rmi nginx
```

---

### Mistake

Thinking `docker ps -a` shows images.

Reality:

- `docker ps -a` shows containers.
- `docker images` shows images.

## Docker Volume Mistakes

### Mistake

Assuming container data persists after container deletion.

Reality:

Data stored in the container's writable layer is deleted with the container.

Use Docker volumes for persistent data.

---

### Mistake

Trying to remove a volume while it is still being used by a container.

Correct approach:

1. Remove the container.
2. Remove the volume.

## Bind Mount Mistakes

### Mistake

Using the wrong host path while creating a bind mount.

Incorrect:

```bash
-v ~/bind-demo:/usr/share/nginx/html
```

Correct:

```bash
-v ~/DevOps/docker/docker-learnings/bind-demo:/usr/share/nginx/html
```

---

### Mistake

Thinking bind mounts copy files into the container.

Reality:

The container accesses the same files directly from the host machine.
