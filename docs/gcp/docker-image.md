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
