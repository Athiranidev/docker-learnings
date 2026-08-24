# Lab 13 - Docker Networking

## Objective

Understand Docker bridge networks, user-defined networks, Docker DNS, and
container-to-container communication.

## Step 1 - List Networks

Command:

    docker network ls

Default Docker networks include:

    bridge
    host
    none

A custom network may also be present.

## Step 2 - Inspect the Default Bridge

Command:

    docker network inspect bridge

The network contains information such as:

- Subnet
- Gateway
- Driver
- Connected containers

## Step 3 - Create a User-Defined Network

Command:

    docker network create demo-network

## Step 4 - Create the Application Container

Command:

    docker run -dit --name app-server --network demo-network ubuntu bash

## Step 5 - Create the Client Container

Command:

    docker run -dit --name client --network demo-network ubuntu bash

Both containers are connected to:

    demo-network

## Step 6 - Test Docker DNS

Enter the client:

    docker exec -it client bash

Run:

    getent hosts app-server

Example result:

    172.19.0.2    app-server

This proves that Docker DNS can resolve the container name on the
user-defined network.

## Step 7 - Test Nginx on app-server

Enter the application container:

    docker exec -it app-server bash

Install Nginx:

    apt update
    apt install -y nginx

Start Nginx:

    nginx

Test Nginx configuration:

    nginx -t

Expected result:

    syntax is ok
    test is successful

## Step 8 - Test Container-to-Container Communication

Enter the client:

    docker exec -it client bash

Install curl:

    apt update
    apt install -y curl

Test:

    curl http://app-server

The Nginx welcome page was returned successfully.

## Important Observation

No host port publishing was required.

We did not use:

    -p 8080:80

because the communication was:

    client -> app-server

and both containers were connected to the same user-defined Docker network.

## localhost vs Container Name

Inside the client:

    localhost

refers to the client container itself.

To access the application container:

    http://app-server

Docker resolves `app-server` to its container IP.

## Troubleshooting

If container-to-container communication fails:

1. Check that both containers are running:

       docker ps

2. Check the network:

       docker network inspect demo-network

3. Check DNS resolution:

       getent hosts app-server

4. Check application logs:

       docker logs app-server

5. Check whether the application is listening on the expected port.

6. Test the application:

       curl http://app-server

## Result

Successfully demonstrated:

- Docker networks
- Default bridge network
- User-defined bridge network
- Docker DNS
- Container name resolution
- Container-to-container communication
- Nginx running inside a container
- HTTP communication between containers
- Difference between localhost and container names
- Difference between internal container communication and host port publishing