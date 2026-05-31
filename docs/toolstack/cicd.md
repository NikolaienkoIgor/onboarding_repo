# CI/CD

At Fintom8 we practise **Continuous Integration and Continuous Delivery (CI/CD)**. Every commit triggers automated pipelines that build, test, and deploy our code — keeping the path from a developer's laptop to production fast, reliable, and auditable.

---

## Overview

```
Developer pushes code
       │
       ▼
  GitHub Actions (CI)
  ┌─────────────────────────────────┐
  │  1. Lint & format check         │
  │  2. Unit & integration tests    │
  │  3. Security scan (Trivy/Snyk)  │
  │  4. Build Docker image          │
  │  5. Push image to ECR           │
  └─────────────────────────────────┘
       │  (on merge to main)
       ▼
   ArgoCD (CD)
  ┌───────────────────────────────┐
  │  GitOps sync to Kubernetes    │
  │  Staging → Canary → Prod      │
  └───────────────────────────────┘
```

---

## GitHub Actions

All CI pipelines are defined as YAML workflow files inside `.github/workflows/` in each repository.

### Anatomy of a workflow file

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Run tests
        run: pytest --cov

      - name: Lint
        run: ruff check .
```

### Key workflow files you'll encounter

| File | Purpose |
|------|---------|
| `ci.yml` | Lint, test, security scan on every PR |
| `build.yml` | Build and push Docker image on merge to `main` |
| `deploy-staging.yml` | Deploy to staging environment |
| `deploy-prod.yml` | Deploy to production (manual approval required) |

!!! info "PR checks"
    All checks in `ci.yml` must pass before a PR can be merged. If a check fails, fix it before requesting review.

---

## ArgoCD (GitOps)

We use **ArgoCD** for continuous delivery. It watches our infrastructure Git repositories and automatically syncs the desired state to our Kubernetes clusters.

- Dashboard: `https://argocd.fintom8.internal`
- Access: request via **#it-helpdesk**
- All deployments are triggered by a Git commit to the infra repo — no manual `kubectl apply`.

### Deployment environments

| Environment | Trigger | URL |
|-------------|---------|-----|
| **Staging** | Merge to `develop` | `https://staging.fintom8.com` |
| **Production** | Manual approval after staging | `https://app.fintom8.com` |

---

## Secrets management

- **Never** hardcode secrets in code or workflow files.
- Use **GitHub Secrets** (`Settings → Secrets`) for CI/CD pipeline credentials.
- Use **AWS Secrets Manager** for application runtime secrets.
- Use **1Password** for sharing secrets among team members.

---

## Monitoring deployments

After a deployment, check these dashboards to confirm everything is healthy:

- **Grafana** — metrics and dashboards: `https://grafana.fintom8.internal`
- **Datadog** — logs, traces, APM: `https://app.datadoghq.com`
- **Slack `#alerts`** — automated alerts from PagerDuty and CloudWatch

!!! warning "Rollbacks"
    If a deployment causes issues, notify **#engineering** immediately. ArgoCD supports one-click rollback to the previous Git revision.
