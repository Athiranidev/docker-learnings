# Docker Interview Questions

## 1. What is Docker?

**Answer:**

Docker is an open-source containerization platform that packages an application along with its dependencies into containers, ensuring it runs consistently across different environments.

---

## 2. Why do we use Docker?

**Answer:**

Docker solves the "It works on my machine" problem by providing a consistent runtime environment from development to production.

---

## 3. What is Containerization?

**Answer:**

Containerization is the process of packaging an application along with its dependencies into an isolated container so it can run consistently on different systems.

---

## 4. What is a Docker Image?

**Answer:**

A Docker Image is a read-only template containing an application, its runtime, libraries, dependencies, and configuration.

---

## 5. What is a Docker Container?

**Answer:**

A Docker Container is a running instance of a Docker Image.

---

## 6. Difference between Image and Container?

| Image | Container |
|--------|-----------|
| Blueprint | Running instance |
| Read-only | Writable layer |
| Used to create containers | Executes the application |

---

## 7. Docker vs Virtual Machine?

| Docker | Virtual Machine |
|---------|-----------------|
| Shares host kernel | Has its own kernel |
| Lightweight | Heavy |
| Starts in seconds | Takes longer to boot |
| Uses fewer resources | Uses more resources |

---

## 8. Explain the command:

```bash
docker run -it ubuntu bash
docker → Docker CLI
run → Creates and starts a new container
-i → Interactive mode
-t → Allocates a terminal
ubuntu → Image name
bash → Command executed inside the container

