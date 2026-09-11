# Lab 15 — Docker Multi-stage Build

## Objective

Compare a single-stage Docker build with a multi-stage Docker build and understand how multi-stage builds reduce the final image size.

---

## Step 1 — Create the Application

Created `main.go`:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello from a multi-stage Docker build!")
}

Step 2 — Single-stage Dockerfile
FROM golang:1.25

WORKDIR /app

COPY main.go .

RUN go build -o app main.go

CMD ["./app"]

Build:

docker build -t go-single-stage:v1 .

Check size:

docker images go-single-stage:v1

Result:

Disk usage: 1.29GB
Content size: 314MB

The image contains the Go build environment even though it is not required to run the application.

Step 3 — Multi-stage Dockerfile

Created Dockerfile.multistage:

FROM golang:1.25 AS builder

WORKDIR /app

COPY main.go .

RUN go build -o app main.go

FROM alpine:latest

WORKDIR /app

COPY --from=builder /app/app .

CMD ["./app"]

Build:

docker build -f Dockerfile.multistage -t go-multistage:v1 .

Check size:

docker images go-multistage:v1

Result:

Disk usage: 16.5MB
Content size: 5.2MB
Step 4 — Run the Multi-stage Image
docker run --rm go-multistage:v1

Output:

Hello from a multi-stage Docker build!

The application successfully ran from the small runtime image.

Step 5 — Understand --rm

The command:

docker run --rm go-multistage:v1

automatically removes the container after the main process exits.

Therefore, the temporary container does not appear in:

docker ps -a

The image itself remains available.

Comparison

Build	Image	Disk Usage
Single-stage	go-single-stage:v1	1.29GB
Multi-stage	go-multistage:v1	16.5MB