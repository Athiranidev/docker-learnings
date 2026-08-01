# Lab 04 - Running Nginx Container and Docker Logs

## Objective

Learn how to:

- Pull a Docker image
- Run an Nginx container
- Expose the container using port mapping
- Access the application from the browser
- View container logs
- Understand host ports and container ports

---

## Environment

- OS: Ubuntu 24.04 LTS
- Docker Engine
- Image: nginx:latest

---

## Step 1 - Pull the Nginx Image

```bash
docker pull nginx
```

### Observation

- Downloaded the latest Nginx image from Docker Hub.
- Image stored locally.

---

## Step 2 - Verify Images

```bash
docker images
```

### Observation

- Verified that the nginx image exists locally.

---

## Step 3 - Run the Nginx Container

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

### Observation

- Container started successfully.
- Running in detached mode.
- Host port 8080 mapped to container port 80.

---

## Step 4 - Verify Running Containers

```bash
docker ps
```

### Observation

- Container status: Up
- Port mapping displayed as:

```
0.0.0.0:8080->80/tcp
```

---

## Step 5 - Access the Application

Open:

```
http://localhost:8080
```

### Observation

- Successfully displayed the **Welcome to nginx!** page.

---

## Step 6 - View Container Logs

```bash
docker logs my-nginx
```

### Observation

- Viewed Nginx startup logs.
- Verified worker processes started.
- Observed HTTP requests after opening the browser.
- Observed a 404 error for favicon.ico, which is normal.

---

## Step 7 - View Live Logs

```bash
docker logs -f my-nginx
```

### Observation

- Logs were displayed in real time.
- Refreshing the browser generated new GET requests.

---

## Step 8 - Run Another Nginx Container

```bash
docker run -d --name nginx2 -p 8081:80 nginx
```

### Observation

- Second container started successfully.
- Used a different host port (8081).
- Both containers ran simultaneously.

---

## Key Learnings

- Docker images are downloaded using `docker pull`.
- `docker run` creates and starts a container.
- `-d` runs the container in the background.
- `-p` maps a host port to a container port.
- Multiple containers can use the same container port.
- Host ports must be unique.
- `docker logs` is used for troubleshooting applications.
- `docker logs -f` displays logs in real time.

---

## Commands Practiced

```bash
docker pull nginx
docker images
docker run -d --name my-nginx -p 8080:80 nginx
docker ps
docker logs my-nginx
docker logs -f my-nginx
docker run -d --name nginx2 -p 8081:80 nginx
```