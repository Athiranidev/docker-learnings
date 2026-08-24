# Docker Networking

## What is Docker Networking?

Docker networking allows containers to communicate with:

- Other containers
- The Docker host
- External networks and services

## Docker Network Types

Common Docker network drivers:

- bridge
- host
- none
- overlay
- macvlan
- ipvlan

## Bridge Network

Bridge is the common network driver for containers running on the same Docker host.

Docker creates a default `bridge` network automatically.

A user-defined bridge network can also be created for an application.

## Default Bridge vs User-Defined Bridge

Default bridge:

- Automatically used when no network is specified.
- Containers can communicate using IP addresses.
- Container-name DNS resolution is not provided in the same way as user-defined bridge networks.

User-defined bridge:

- Created using `docker network create`.
- Provides better isolation.
- Containers can communicate using container names.
- Docker provides automatic DNS resolution between containers.

Example:

    docker network create demo-network

    docker run -dit --name app-server --network demo-network ubuntu bash

    docker run -dit --name client --network demo-network ubuntu bash

The client can resolve:

    app-server

to the container's internal IP address.

## Container-to-Container Communication

Containers connected to the same user-defined network can communicate directly.

Example:

    client
       |
       | http://app-server:80
       |
       v
    app-server

The application does not need to publish its port to the host for another
container on the same Docker network to access it.

## Container Name vs localhost

Inside a container:

    localhost

refers to the current container itself.

It does not refer to another container.

For example:

    http://localhost:8080

means port 8080 inside the current container.

To communicate with another container on the same network:

    http://backend:8080

where `backend` is the container/service name.

## Port Publishing

Port publishing maps a container port to a port on the Docker host.

Example:

    docker run -p 8080:80 nginx

Meaning:

    Host port 8080
          |
          v
    Container port 80

Port publishing is required when a service needs to be accessed through the
Docker host or from outside the Docker network.

It is not required merely for communication between containers on the same
user-defined network.

## Docker DNS

User-defined networks provide Docker's embedded DNS service.

For example:

    client -> app-server

Docker resolves:

    app-server -> container IP

This is preferable to hard-coding container IP addresses because container
IP addresses can change when containers are recreated.

## Network Isolation

Containers attached to different user-defined networks are isolated from
each other by default.

Example:

    frontend-network
        |
        frontend

    backend-network
        |
        database

A container must be connected to the appropriate network to communicate with
another container.

## Network Troubleshooting

When a container cannot communicate with another service, check:

1. Is the container running?

       docker ps

2. Are both containers connected to the same network?

       docker network inspect <network>

3. Can the destination name be resolved?

       getent hosts <container-name>

4. Is the application actually running?

       docker logs <container>

5. Is the application listening on the expected container port?

6. Can the client connect to the service?

       curl http://<container-name>:<port>

DNS resolution alone does not prove that an application is listening. It only
proves that the name can be resolved to an address.