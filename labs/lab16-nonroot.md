# Lab 16 — Docker Security & Minimal Runtime Images

## 1. Non-root Container

Containers may run as root (UID 0) by default.

Check:

docker run --rm -it ubuntu bash
whoami
id

Better practice:

FROM alpine:latest
RUN adduser -D appuser
USER appuser
CMD ["whoami"]

Result:
appuser
uid=1000(appuser)

Purpose:
Run the application with minimum required privileges.

Mental model:
USER → Who can run?

---

## 2. Linux Capabilities

Capabilities are individual Linux privileges.

Example:

docker run --rm --cap-drop=NET_RAW alpine sh

Stronger restriction:

docker run --rm --cap-drop=ALL --cap-add=NET_BIND_SERVICE alpine

Principle:
Least privilege — give the container only the capabilities it needs.

Mental model:

USER → Who?
CAPABILITIES → What privileges?

---

## 3. Read-only Filesystem

Make the container filesystem read-only:

docker run --rm -it --read-only alpine sh

For a temporary writable /tmp:

docker run --rm -it --read-only --tmpfs /tmp alpine sh

Mental model:

--read-only → Prevent normal filesystem writes
--tmpfs /tmp → Temporary writable memory-backed storage

tmpfs data disappears when the container is removed.

---

## 4. Environment Variables

Environment variables are normally used for application configuration.

Example:

docker run -e APP_ENV=production myapp

Examples:

APP_ENV
PORT
LOG_LEVEL

Use environment variables for normal configuration.

---

## 5. Docker Secrets

Secrets are sensitive information such as:

Passwords
API keys
Tokens
Private keys

Docker Compose can mount a secret inside the container:

/run/secrets/db_password

Example:

services:
  app:
    image: alpine:latest
    command: ["sh", "-c", "cat /run/secrets/db_password && sleep 300"]
    secrets:
      - db_password

secrets:
  db_password:
    file: ./db_password.txt

Important:
Do not commit real secrets to GitHub.

The lab password file is excluded using .gitignore.

Mental model:

Environment variable → Normal configuration
Secret → Sensitive data

---

## 6. Docker Scout Vulnerability Scanning

Scan an image:

docker scout cves <image>

Example:

docker scout cves go-multistage:v1

Get recommendations:

docker scout recommendations <image>

Security workflow:

BUILD
  ↓
SCAN
  ↓
FIND VULNERABILITY
  ↓
FIX
  ↓
REBUILD
  ↓
SCAN AGAIN

Important:
A small image is not automatically a secure image.

---

## 7. Multi-stage Build

Multi-stage builds separate the build environment from the runtime environment.

Example:

FROM golang:1.25 AS builder

WORKDIR /app

COPY main.go .

RUN go build -o app main.go

FROM alpine:latest

WORKDIR /app

COPY --from=builder /app/app .

CMD ["./app"]

The Go compiler and build tools stay in the builder stage.

Only the compiled application is copied into the final runtime image.

Principle:

Build with everything you need;
Run with only what you need.

Benefits:

- Smaller image
- Smaller runtime attack surface
- Build tools are not shipped to production

But:
Multi-stage builds do not guarantee zero vulnerabilities.

---

## 8. Choosing a Base Image

Do not choose a base image only because it is small.

Ask:

What does my application need at runtime?
    ↓
Does it need an interpreter?
Does it need OS libraries?
Does it need libc?
Does it need a shell?
    ↓
Choose the smallest suitable,
compatible and secure image.

Examples:

Go static binary → scratch may work
Python → Python runtime image
Node.js → Node runtime image
Nginx → Nginx image

Important:

Base image selection depends on:
- Runtime requirements
- Compatibility
- Security
- Size
- Maintainability

---

## 9. Alpine vs Scratch

We tested the same Go application using Alpine and Scratch.

Alpine:

go-multistage:v1
Disk usage: 16.5 MB
Content size: 5.2 MB
Vulnerabilities: 2 Critical, 7 High, 1 Medium

Scratch:

go-scratch:v1
Disk usage: 3.62 MB
Content size: 1.36 MB
Vulnerabilities detected: 0

Scratch application test:

docker run --rm go-scratch:v1

Output:

Hello from a multi-stage Docker build!

Scratch worked because the Go application was compiled into a self-contained executable.

Dockerfile:

FROM scratch

COPY --from=builder /app/app /app

CMD ["/app"]

---

## 10. Scratch Limitation

Scratch contains essentially no normal Linux userspace utilities.

Trying:

docker run --rm -it go-scratch:v1 sh

fails because sh does not exist.

Scratch does not normally contain:

sh
bash
ls
cat
curl
wget
apk

Alpine provides a normal small Linux userspace, so debugging is easier.

Trade-off:

Scratch:
- Very small
- Minimal runtime attack surface
- Fewer OS packages
- Difficult to debug

Alpine/slim:
- Larger
- More runtime components
- Easier debugging
- More packages to maintain and scan

Therefore:

Smallest image ≠ Always the best image.

Choose the smallest image that provides everything the application actually needs.

---

## 11. Dockerfile Filename vs Base Image

Dockerfile name does not determine the base image.

Example:

Dockerfile
    ↓
FROM alpine:latest

Dockerfile.scratch
    ↓
FROM scratch

The FROM instruction determines the base image.

Multiple Dockerfiles can be distinguished using filenames:

Dockerfile
Dockerfile.scratch
Dockerfile.alpine

Build a specific Dockerfile:

docker build -f Dockerfile.scratch -t go-scratch:v1 .

---

## 12. Important Security Mental Model

USER
  ↓
Who can run?

CAPABILITIES
  ↓
What privileges?

--read-only
  ↓
Where can it write?

SECRETS
  ↓
How is sensitive data protected?

MINIMAL BASE IMAGE
  ↓
What unnecessary components can be removed?

VULNERABILITY SCAN
  ↓
What known vulnerabilities remain?



## Key Takeaways

1. Run containers as non-root whenever possible.
2. Use Linux capabilities according to least privilege.
3. Use read-only filesystems where practical.
4. Use environment variables for normal configuration.
5. Use secrets for sensitive information.
6. Scan images for known vulnerabilities.
7. Multi-stage builds remove unnecessary build tools.
8. Choose the base image according to runtime requirements.
9. Scratch is suitable for applications that can run without a normal userspace.
10. Scratch provides a smaller runtime attack surface but is harder to debug.
11. Smaller image does not automatically mean secure.
12. Security is layered:

Non-root
+
Capabilities
+
Read-only filesystem
+
Secrets management
+
Minimal runtime image
+
Vulnerability scanning