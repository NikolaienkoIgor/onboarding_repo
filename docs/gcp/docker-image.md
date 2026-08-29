# Docker Image

!!! tip
    [Docker](https://docs.docker.com/get-docker/) must already be installed before running these steps.

---

## 1. Authenticate Docker with GCP

```bash
gcloud auth configure-docker
```

Lets Docker pull images from Google Container Registry / Artifact Registry.

---

## 2. Pull the Docker Image

```bash
docker pull IMAGE_NAME:TAG
```

Replace `IMAGE_NAME:TAG` with the actual image path (e.g. `us-docker.pkg.dev/PROJECT/REPO/IMAGE:latest`).

### Verify

```bash
docker images
```

---

## 3. Run the Container

```bash
docker run -d --name CONTAINER_NAME -p HOST_PORT:CONTAINER_PORT IMAGE_NAME:TAG
```

- `-d` runs the container in the background (detached mode)
- `--name` gives the container a memorable name instead of a random one
- `-p HOST_PORT:CONTAINER_PORT` maps a port on your machine to a port inside the container

### Verify it's running

```bash
docker ps
```

Shows all currently running containers. Add `-a` to also see stopped ones:

```bash
docker ps -a
```

---

## 4. Basic Docker Commands

A quick reference for the commands you'll use day-to-day once an image is pulled and a container is running.

### Starting and stopping containers

```bash
docker start CONTAINER_NAME_OR_ID
```

Starts a container that already exists but is stopped.

```bash
docker stop CONTAINER_NAME_OR_ID
```

Stops a running container gracefully.

```bash
docker restart CONTAINER_NAME_OR_ID
```

Stops and starts the container again — useful after changing environment variables or config.

### Viewing containers and images

```bash
docker ps -a
```

Lists all containers, running or stopped.

```bash
docker images
```

Lists all locally pulled images.

### Logs and debugging

```bash
docker logs CONTAINER_NAME_OR_ID
```

Shows the container's output/logs.

```bash
docker logs -f CONTAINER_NAME_OR_ID
```

Follows the logs in real time (like `tail -f`).

```bash
docker exec -it CONTAINER_NAME_OR_ID bash
```

Opens an interactive shell inside a running container — useful for poking around or debugging. Use `sh` instead of `bash` if the image doesn't have bash installed.

### Cleaning up

```bash
docker rm CONTAINER_NAME_OR_ID
```

Removes a stopped container. Add `-f` to force-remove a running one.

```bash
docker rmi IMAGE_NAME:TAG
```

Removes an image you no longer need.

```bash
docker system prune
```

Cleans up unused containers, networks, and dangling images to free up disk space.

!!! tip "Quick sanity check"
    If something feels off, `docker ps -a` (what's running/stopped) and `docker logs CONTAINER_NAME` (what happened) are the first two commands to reach for.
