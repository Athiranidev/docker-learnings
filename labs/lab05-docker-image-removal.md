# Lab 05 - Docker Image Removal

## Objective

Learn how to remove Docker images and understand image-container dependency.

---

## Commands Executed

### List Images

```bash
docker images
```

### Attempt to Remove an Image

```bash
docker rmi nginx
```

### Observation

Docker returned a conflict error because existing containers were still using the image.

---

### Remove Containers

```bash
docker rm my-nginx nginx1 nginx2
```

### Remove Image

```bash
docker rmi nginx
```

### Observation

Image was removed successfully after all containers referencing it were deleted.

---

## Key Learnings

- `docker rm` removes containers.
- `docker rmi` removes images.
- Images cannot be removed while referenced by existing containers.
- Use `docker images` to verify available images.