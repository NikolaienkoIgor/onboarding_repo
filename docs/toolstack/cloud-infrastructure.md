# Cloud & Infrastructure

Fintom8 runs its infrastructure on **Amazon Web Services (AWS)**. We manage infrastructure as code using **Terraform**, containerise workloads with **Docker**, and orchestrate them with **Kubernetes (EKS)**.

---

## AWS

### Account access

AWS access is managed through **AWS IAM Identity Center (SSO)**.

1. Log in at `https://fintom8.awsapps.com/start`
2. Select the account and role appropriate for your team.
3. For CLI access, configure SSO with:

```bash
aws configure sso
# Follow the prompts — use profile name "fintom8-dev"

# Then use the profile
aws s3 ls --profile fintom8-dev
```

### Key AWS services we use

| Service | What we use it for |
|---------|-------------------|
| **EKS** | Managed Kubernetes — runs all our workloads |
| **RDS (PostgreSQL)** | Relational databases |
| **ElastiCache (Redis)** | Caching and session storage |
| **S3** | Object storage: backups, uploads, static assets |
| **SQS / SNS** | Async messaging and event-driven workflows |
| **ECR** | Private Docker image registry |
| **Secrets Manager** | Runtime secret injection |
| **CloudWatch** | Logs and metrics |
| **Route 53** | DNS management |

!!! warning "Cost awareness"
    Be mindful of the resources you spin up in development accounts. Delete temporary resources when done, and tag everything with your name and the Jira ticket number.

---

## Terraform

All infrastructure is defined as code in the `fintom8/infra` repository. We use **Terraform** to provision and manage AWS resources.

### Basic workflow

```bash
# Navigate to a module
cd infra/environments/staging

# Initialise providers and backend
terraform init

# Preview changes
terraform plan

# Apply changes (requires peer review + merge to main first)
terraform apply
```

!!! danger "No manual changes"
    Never make manual changes to infrastructure in the AWS console. All changes must go through Terraform and a reviewed PR. The CI pipeline enforces this with drift detection.

### Module structure

```
infra/
├── modules/          # Reusable Terraform modules
│   ├── eks-cluster/
│   ├── rds-postgres/
│   └── s3-bucket/
├── environments/
│   ├── staging/      # Staging environment root module
│   └── production/   # Production environment root module
└── shared/           # Shared resources (DNS, IAM roles)
```

---

## Docker

Every application is containerised using **Docker**.

### Common commands

```bash
# Build an image
docker build -t my-app:local .

# Run a container locally
docker run -p 8080:8080 --env-file .env my-app:local

# View running containers
docker ps

# View logs
docker logs <container-id>

# Open a shell in a running container
docker exec -it <container-id> /bin/sh
```

### Dockerfile conventions

- Use official base images from Docker Hub or AWS ECR Public Gallery.
- Pin image versions — never use `latest` in production.
- Use multi-stage builds to keep images small.
- Run processes as a non-root user.

---

## Kubernetes (EKS)

Our applications run on **Amazon EKS (Elastic Kubernetes Service)**. You'll interact with clusters via `kubectl`.

### Setup

```bash
# Configure kubectl for the staging cluster
aws eks update-kubeconfig \
  --name fintom8-staging \
  --region eu-central-1 \
  --profile fintom8-dev
```

### Useful kubectl commands

```bash
# List pods in your team's namespace
kubectl get pods -n <your-namespace>

# Stream logs from a pod
kubectl logs -f <pod-name> -n <your-namespace>

# Describe a pod (useful for debugging CrashLoopBackOff)
kubectl describe pod <pod-name> -n <your-namespace>

# Port-forward a service to your laptop
kubectl port-forward svc/<service-name> 8080:80 -n <your-namespace>

# Open a shell inside a running pod
kubectl exec -it <pod-name> -n <your-namespace> -- /bin/sh
```

!!! info "Read-only in production"
    Engineers have read-only access to the production cluster. All production changes go through ArgoCD and require an approved PR.

---

## Local development

For local development we use **Docker Compose** to spin up the full service stack (app + database + Redis) on your laptop.

```bash
# Start all services
docker compose up -d

# Tail logs
docker compose logs -f

# Stop and remove containers
docker compose down
```

Each repository contains a `docker-compose.yml` at the root. Check the repo's `README.md` for any additional setup steps.
