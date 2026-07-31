# Lab 02 - Ubuntu Interactive Container

## Objective

Launch an Ubuntu container and interact with it using the Bash shell.

---

## Command

```bash
docker run -it ubuntu bash
```

---

## Commands Executed

```bash
pwd
whoami
hostname
cat /etc/os-release
ls /
```

---

## Observations

- The default user was `root`.
- The hostname was the container ID.
- Ubuntu was running inside the container.
- The container had its own filesystem.
- The container shared the host Linux kernel.

---

## Learning Outcome

After completing this lab, I learned:

- How to enter a container.
- The difference between the host system and a container.
- Containers are isolated user-space environments.
- Docker containers share the host kernel instead of running their own operating system.