# Docker Multi-stage Builds

## What is a Multi-stage Build?

A multi-stage Docker build uses multiple `FROM` instructions in one Dockerfile.

The application is built in one stage and only the required build artifacts are copied into the final runtime stage.

Basic structure:

Builder stage
↓
Build application
↓
Application artifact
↓
Runtime stage
↓
Final image

---

## Why Use Multi-stage Builds?

Build environments often contain tools that are not required at runtime.

For example, a Go builder image contains:

- Go compiler
- Go development tools
- Build dependencies
- Source code

The final application only needs the compiled binary.

Multi-stage builds allow us to keep the build environment out of the final image.

Benefits:

- Smaller images
- Faster image transfer
- Smaller attack surface
- Cleaner production images
- Separation of build and runtime environments

---

## `AS` — Naming a Build Stage

Example:

FROM golang:1.25 AS builder

The `AS builder` part gives the build stage a name.

---

## `COPY --from`

Example:

COPY --from=builder /app/app .

This copies the required file from the `builder` stage into the current stage.

The compiler and other builder-stage files are not copied into the final image.

---

## Single-stage vs Multi-stage

Single-stage:

FROM golang:1.25

The final image contains the Go build environment and application.

Multi-stage:

FROM golang:1.25 AS builder

Build the application.

FROM alpine:latest

Copy only the compiled application.

---

## Practical Result

For our Go application:

Single-stage image:

go-single-stage:v1
Disk usage: 1.29GB

Multi-stage image:

go-multistage:v1
Disk usage: 16.5MB

The multi-stage image was dramatically smaller because the Go build environment was excluded from the final image.

---

## Mental Model

Build with everything you need.

Run with only what you need.

Builder:
Source + compiler + build tools
↓
Application artifact
↓
Runtime:
Minimal runtime + application artifact