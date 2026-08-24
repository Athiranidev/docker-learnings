# Lab 12 - Docker ENTRYPOINT and CMD

## Objective

Understand how ENTRYPOINT and CMD work individually and together, and practice
overriding CMD and ENTRYPOINT during container execution.

## Step 1 - Create Dockerfile

Dockerfile:

    FROM ubuntu

    ENTRYPOINT ["echo"]

    CMD ["Hello Docker"]

## Step 2 - Build Image

    docker build -t entrypoint-demo:v1 .

## Step 3 - Run Default Command

    docker run --rm entrypoint-demo:v1

Output:

    Hello Docker

The ENTRYPOINT was:

    echo

The default CMD was:

    Hello Docker

The effective command was:

    echo "Hello Docker"

## Step 4 - Override CMD

    docker run --rm entrypoint-demo:v1 "Hello DevOps"

Output:

    Hello DevOps

The ENTRYPOINT remained:

    echo

The CMD argument was replaced with:

    Hello DevOps

Effective command:

    echo "Hello DevOps"

## Step 5 - Test CMD Only

Dockerfile:

    FROM ubuntu

    CMD ["echo", "Hello Docker"]

Build:

    docker build -t cmd-demo:v1 .

Run:

    docker run --rm cmd-demo:v1

Output:

    Hello Docker

Override the CMD:

    docker run --rm cmd-demo:v1 echo "Hello DevOps"

Output:

    Hello DevOps

The complete CMD was replaced by the runtime command.

## Step 6 - Override ENTRYPOINT

Using:

    docker run --entrypoint printf entrypoint-demo:v2 "Hello %s\n" DevOps

Output:

    Hello DevOps

The image's ENTRYPOINT was temporarily replaced with:

    printf

The container executed:

    printf "Hello %s\n" DevOps

## Step 7 - Check Container State

Without --rm:

    docker run --entrypoint printf entrypoint-demo:v2 "Hello %s\n" DevOps

Check:

    docker ps

The container was not shown because it had already exited.

Check all containers:

    docker ps -a

The stopped container was visible with:

    Exited (0)

Docker also generated a container name because --name was not specified.

## Step 8 - Troubleshooting Scenario

Scenario:

A container exits immediately with:

    Exited (0)

Dockerfile:

    FROM ubuntu

    COPY app.sh /app.sh

    ENTRYPOINT ["/app.sh"]

    CMD ["start"]

The effective startup command is:

    /app.sh start

If /app.sh completes and exits, the container also exits.

Troubleshooting approach:

    docker ps -a
    docker logs <container>

Then investigate the ENTRYPOINT and CMD and determine why the main process
exited.

The container should not be restarted immediately without understanding the
cause.

## Step 9 - Override ENTRYPOINT for Troubleshooting

A shell can be used to bypass the image ENTRYPOINT:

    docker run --rm -it --entrypoint bash <image>

This allows the container filesystem and application files to be inspected.

## Result

Successfully practiced:

- ENTRYPOINT
- CMD
- ENTRYPOINT + CMD
- Overriding CMD
- Overriding ENTRYPOINT
- --entrypoint
- --rm
- Container exit status
- Image name vs container name
- Basic ENTRYPOINT troubleshooting