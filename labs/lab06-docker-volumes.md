# Lab 06 - Docker Volumes

## Objective

Learn how Docker volumes provide persistent storage.

---

## Commands Executed

### Create a Volume

```bash
docker volume create my-volume
```

### List Volumes

```bash
docker volume ls
```

### Inspect Volume

```bash
docker volume inspect my-volume
```

### Run Container with Volume

```bash
docker run -it --name ubuntu-volume \
-v my-volume:/data \
ubuntu bash
```

### Create a File

```bash
cd /data
echo "Hello Docker Volume" > file1.txt
```

### Remove Container

```bash
docker rm ubuntu-volume
```

### Create Another Container Using Same Volume

Verified that `file1.txt` still existed.

### Remove Volume

```bash
docker volume rm my-volume
```

---

## Key Learnings

- Docker volumes provide persistent storage.
- Volumes exist independently of containers.
- Data survives container deletion.
- Volumes can be inspected and removed using Docker volume commands.