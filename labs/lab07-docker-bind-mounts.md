# Lab 07 - Docker Bind Mounts

## Objective

Learn how Docker Bind Mounts allow a container to access files directly from the host machine.

---

## Commands Executed

### Create a Demo Directory

```bash
mkdir bind-demo
echo "Hello from Host Machine" > bind-demo/index.html
```

### Run Nginx with a Bind Mount

```bash
docker run -d \
--name nginx-bind \
-p 8082:80 \
-v ~/DevOps/docker/docker-learnings/bind-demo:/usr/share/nginx/html \
nginx
```

### Verify in Browser

```
http://localhost:8082
```

### Modify the Host File

Updated `index.html` on the host machine and refreshed the browser.

### Verify Inside the Container

```bash
docker exec -it nginx-bind bash
cat /usr/share/nginx/html/index.html
```

---

## Observations

- Changes made on the host were immediately visible inside the container.
- No container restart was required.
- The container accessed the same files stored on the host.

---

## Key Learnings

- Bind mounts share host files directly with containers.
- They are ideal for local development.
- Host file changes are instantly reflected inside the container.