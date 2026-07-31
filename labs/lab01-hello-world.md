# Lab 01 - Hello World

## Objective

Run the first Docker container and verify that Docker is installed and working correctly.

---

## Command

```bash
docker run hello-world
```

---

## What Happens?

1. Docker checks if the `hello-world` image exists locally.
2. If not found, Docker downloads it from Docker Hub.
3. Docker creates a new container from the image.
4. The container executes the program.
5. A success message is displayed.
6. The container exits automatically.

---

## Expected Output

You should see a message similar to:

```
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

---

## Learning Outcome

After completing this lab, I learned:

- Docker can download images automatically.
- Docker creates a container from an image.
- Containers can execute a task and exit.
- Docker installation is working correctly.