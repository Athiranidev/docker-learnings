# Lab 11 - Dockerized Nginx Application

## Objective

Build a custom Nginx Docker image and serve a custom HTML page through the container.

## Dockerfile

    FROM nginx

    WORKDIR /app

    ENV APP_ENV=production

    COPY index.html /usr/share/nginx/html/index.html

    EXPOSE 80

## Build

    docker build -t my-nginx:v1 .

The image was successfully created.

## Run

    docker run -d --name my-nginx -p 8081:80 my-nginx:v1

The container started successfully.

## Verify Inside Container

    docker exec -it my-nginx bash

Checked:

    cat /usr/share/nginx/html/index.html

Output:

    Hello from my Dockerized application

## Verify From Host

    curl http://localhost:8081

Output:

    Hello from my Dockerized application

## Troubleshooting

The browser initially displayed the default Nginx welcome page.

The container was inspected and the custom index.html was confirmed to exist inside:

    /usr/share/nginx/html/index.html

The application was then tested using curl:

    curl http://localhost:8081

The correct custom content was returned.

This confirmed that the Docker container, Nginx and port mapping were working correctly. The browser was displaying stale content.

## Result

Successfully built, deployed and verified a custom Nginx application using Docker.