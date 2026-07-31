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