# Docker Compose

## What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML file (`compose.yaml`).

It allows us to configure services, images, ports, volumes, networks, environment variables, and dependencies in one file.

---

## Basic Structure

```yaml
services:
  web:
    image: nginx
    ports:
      - "8082:80"

```

services → defines application services
web → service name
image → image used by the service
ports → host:container port mapping

Compose Network

Compose automatically creates a project network:

compose-demo_default

Services in the same Compose network can communicate using their service names.

Example:

curl http://web

Here web is the service name.

Networking
client → Compose network → web → port 80 → Nginx

localhost inside a container refers to that same container, not another service.

Bind Mount
volumes:
  - ./index.html:/usr/share/nginx/html/index.html

Maps a host file/directory into the container.

Common use:

Source code
Configuration files
Development
Docker Volume
volumes:
  - app-data:/app/data

Docker-managed persistent storage.

Common use:

Database data
Application data
Easy rule

Source/development files → Bind mount

Persistent application data → Docker volume

Environment Variables
environment:
  APP_ENV: development

Passes runtime environment variables into the container.

Check inside the container:

docker compose exec web env | grep APP_ENV
depends_on
depends_on:
  - web

Controls service startup order.

For example:

web starts
   ↓
client starts

service_started means the container has started, but the application may not be ready.

Healthcheck
healthcheck:
  test: ["CMD", "nginx", "-t"]
  interval: 10s
  timeout: 5s
  retries: 3

Allows Docker to determine whether a container passes a health test.

Check status:

docker compose ps

Example:

Up (healthy)
depends_on + healthcheck
depends_on:
  web:
    condition: service_healthy

This waits for the web healthcheck to pass before starting the dependent service.

web starts
    ↓
healthcheck
    ↓
healthy
    ↓
client starts
Long-running vs One-off Containers
Long-running

Examples:

Nginx
Backend API
PostgreSQL
Redis

Their main process keeps running, so the container remains Up.

One-off

Examples:

curl
database migration
debugging command

The command runs, finishes, and the container exits.

For temporary testing:

docker compose run --rm client curl http://web

--rm removes the temporary container after the command finishes.

Important Compose Commands
docker compose config

Validate and display the resolved Compose configuration.

docker compose up -d

Create resources if needed and start services in detached mode.

docker compose ps

Show running Compose containers.

docker compose ps -a

Show running and stopped containers.

docker compose logs

View service logs.

docker compose stop

Stop containers but keep them.

docker compose start

Start existing stopped containers.

docker compose restart

Restart existing containers.

docker compose down

Stop and remove Compose containers and the project network.