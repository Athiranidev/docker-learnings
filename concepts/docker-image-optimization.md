# Docker Image Optimization

Docker image optimization means reducing unnecessary image size, build context, and build time while keeping the application compatible and maintainable.

## 1. .dockerignore

`.dockerignore` prevents unnecessary files from being sent to the Docker build context.

Example:
*.log
.env
node_modules

Build command:
docker build -t myapp:v1 .

The final `.` means the current directory is the build context.

## 2. COPY . .

COPY . .

First `.` = source from the build context.
Second `.` = destination inside the image.

With WORKDIR /app, it means copying files from the build context into `/app`, except files excluded by `.dockerignore`.

## 3. Layer Caching

Docker builds images in layers. Unchanged layers can be reused from cache.

Example:
FROM alpine:latest
WORKDIR /app
COPY app.txt .
RUN echo "Installing dependencies..."
CMD ["cat", "app.txt"]

If `app.txt` does not change, Docker can reuse the cached layers.

If `app.txt` changes:

WORKDIR → cached
COPY → rebuilt
RUN → rebuilt
Later layers → rebuilt

Therefore, Dockerfile instruction order matters.

Put operations that change less frequently earlier and frequently changing files later.

## 4. Multi-stage Builds

Use a build stage containing compilers and build dependencies, and a separate runtime stage containing only what is required to run the application.

Example:
FROM golang:1.25 AS builder
WORKDIR /app
COPY . .
RUN go build -o app main.go

FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/app .
CMD ["./app"]

This keeps build tools out of the final runtime image.

## 5. Minimal Suitable Base Image

Use a smaller base image when it is compatible with the application.

Examples:
Go static binary → scratch may be suitable
Go application → Alpine may be suitable
Python → Python runtime image
Node.js → Node runtime image
Nginx → Nginx image

The smallest image is not automatically the best image. Consider compatibility, security, maintainability, and required runtime features.

## Image Optimization Summary

.dockerignore
↓
Reduce build context

Good Dockerfile ordering
↓
Improve cache reuse

Multi-stage build
↓
Remove build dependencies

Suitable minimal base image
↓
Reduce final runtime image