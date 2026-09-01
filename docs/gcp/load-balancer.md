# Cross-Project Load Balancer Setup

How to route traffic from a single domain to Cloud Run services in **two separate GCP projects** using a global HTTPS Application Load Balancer.

---

## Goal

| URL | Backend |
|-----|---------|
| `https://example.com/` | Cloud Run `SERVICE_A` (Project A) |
| `https://example.com/app-b/` | Cloud Run `SERVICE_B` (Project B) |

Both apps stay in **separate GCP projects**. A global HTTPS Application Load Balancer in Project A routes by path.

---

## Projects and regions

| Role | Project ID | Region | Cloud Run service |
|------|------------|--------|-------------------|
| Primary (website + LB frontend) | `PROJECT_A` | `REGION_A` | `SERVICE_A` |
| Secondary (app B) | `PROJECT_B` | `REGION_B` | `SERVICE_B` |

Replace the placeholders above with your actual values before running any commands.

---

## Architecture

```
Browser  →  DNS  →  Global static IP (PROJECT_A)
                      │
                      ▼
            Global External HTTPS ALB
            (SSL terminate + URL map)
                      │
          ┌───────────┴───────────┐
          │                       │
   /app-b*                      /* (default)
          │                       │
          ▼                       ▼
Backend (cross-project)   Backend (same project)
PROJECT_B                 PROJECT_A
service-b-backend         service-a-backend
          │                       │
          ▼                       ▼
Serverless NEG            Serverless NEG
REGION_B                  REGION_A
          │                       │
          ▼                       ▼
Cloud Run: SERVICE_B        Cloud Run: SERVICE_A
```

### Design choices

| Choice | Why |
|--------|-----|
| Two projects | Keep services and ownership separate |
| Global external ALB | Cross-project backend referencing **without** Shared VPC |
| Serverless NEG | Bridge ALB → Cloud Run (Cloud Run cannot attach to a normal backend alone) |
| Path-based routing | URL map sends matching paths to the secondary project's backend |

### Glossary

- **NEG (Network Endpoint Group)** — tells the LB where backends live. A *serverless* NEG points at a Cloud Run service.
- **Backend service** — LB config that holds one or more NEGs.
- **URL map** — path/host routing rules.
- **Cross-project reference** — URL map in project A points at a backend service in project B.

---

## Resource inventory

### In `PROJECT_A` (primary)

| Resource | Name |
|----------|------|
| Global static IP | `lb-static-ip` |
| Serverless NEG | `service-a-neg` (`REGION_A`) |
| Backend service | `service-a-backend` |
| Managed SSL cert | `lb-ssl-cert` (`example.com`, `www.example.com`) |
| URL map (HTTPS) | `lb-url-map` |
| URL map (HTTP redirect) | `lb-http-redirect` |
| Target HTTPS proxy | `lb-https-proxy` |
| Target HTTP proxy | `lb-http-proxy` |
| Forwarding rule :443 | `lb-https-forwarding-rule` |
| Forwarding rule :80 | `lb-http-forwarding-rule` |

### In `PROJECT_B` (secondary)

| Resource | Name |
|----------|------|
| Serverless NEG | `service-b-neg` (`REGION_B`) |
| Backend service | `service-b-backend` |
| Cloud Run | `SERVICE_B` |

### IAM

- On `PROJECT_B`: grant `roles/compute.loadBalancerServiceUser` to the user or service account that manages the URL map in `PROJECT_A` (e.g. `user:YOUR_EMAIL` or `serviceAccount:SA_NAME@PROJECT_A.iam.gserviceaccount.com`).

---

## Setup procedure (commands)

Set variables once:

```bash
gcloud auth login

export PROJECT_A=your-primary-project-id
export PROJECT_B=your-secondary-project-id
export REGION_A=your-primary-region        # e.g. europe-west1
export REGION_B=your-secondary-region      # e.g. europe-west3
export SERVICE_A=your-primary-service      # e.g. my-website
export SERVICE_B=your-secondary-service      # e.g. my-app
export DOMAIN=example.com
export PATH_PREFIX=/app-b                    # path routed to SERVICE_B
```

### 0. Prerequisites

```bash
for P in "${PROJECT_A}" "${PROJECT_B}"; do
  gcloud services enable compute.googleapis.com run.googleapis.com --project="${P}"
done
gcloud services enable certificatemanager.googleapis.com --project="${PROJECT_A}"
```

Ensure both Cloud Run services are deployed and healthy before continuing.

### 1. Cross-project IAM

```bash
gcloud projects add-iam-policy-binding "${PROJECT_B}" \
  --member="user:YOUR_EMAIL" \
  --role="roles/compute.loadBalancerServiceUser"
```

Replace `YOUR_EMAIL` with the Google account (or service account) that will create and manage the load balancer in `PROJECT_A`.

### 2. Global static IP (`PROJECT_A`)

```bash
gcloud compute addresses create lb-static-ip \
  --project="${PROJECT_A}" --global --ip-version=IPV4

gcloud compute addresses describe lb-static-ip \
  --project="${PROJECT_A}" --global --format='value(address)'
```

Save the IP address — you will need it for DNS.

### 3A. Primary service NEG + backend (`PROJECT_A`)

```bash
gcloud compute network-endpoint-groups create service-a-neg \
  --project="${PROJECT_A}" --region="${REGION_A}" \
  --network-endpoint-type=serverless --cloud-run-service="${SERVICE_A}"

gcloud compute backend-services create service-a-backend \
  --project="${PROJECT_A}" --global --load-balancing-scheme=EXTERNAL_MANAGED

gcloud compute backend-services add-backend service-a-backend \
  --project="${PROJECT_A}" --global \
  --network-endpoint-group=service-a-neg \
  --network-endpoint-group-region="${REGION_A}"
```

### 3B. Secondary service NEG + backend (`PROJECT_B`)

NEG + backend **must** live in the same project as the Cloud Run service.

```bash
gcloud compute network-endpoint-groups create service-b-neg \
  --project="${PROJECT_B}" --region="${REGION_B}" \
  --network-endpoint-type=serverless --cloud-run-service="${SERVICE_B}"

gcloud compute backend-services create service-b-backend \
  --project="${PROJECT_B}" --global --load-balancing-scheme=EXTERNAL_MANAGED

gcloud compute backend-services add-backend service-b-backend \
  --project="${PROJECT_B}" --global \
  --network-endpoint-group=service-b-neg \
  --network-endpoint-group-region="${REGION_B}"
```

### 4. Managed SSL (`PROJECT_A`)

```bash
gcloud compute ssl-certificates create lb-ssl-cert \
  --project="${PROJECT_A}" \
  --domains="${DOMAIN},www.${DOMAIN}" --global
```

Certificate stays `PROVISIONING` until DNS points at the LB IP.

### 5. URL map (cross-project)

```bash
cat > /tmp/lb-url-map.yaml <<EOF
name: lb-url-map
defaultService: https://www.googleapis.com/compute/v1/projects/${PROJECT_A}/global/backendServices/service-a-backend
hostRules:
- hosts:
  - ${DOMAIN}
  - www.${DOMAIN}
  pathMatcher: lb-paths
pathMatchers:
- name: lb-paths
  defaultService: https://www.googleapis.com/compute/v1/projects/${PROJECT_A}/global/backendServices/service-a-backend
  routeRules:
  - priority: 1
    matchRules:
    - prefixMatch: ${PATH_PREFIX}
    routeAction:
      weightedBackendServices:
      - backendService: https://www.googleapis.com/compute/v1/projects/${PROJECT_B}/global/backendServices/service-b-backend
        weight: 100
EOF

gcloud compute url-maps import lb-url-map \
  --project="${PROJECT_A}" --source=/tmp/lb-url-map.yaml --global --quiet
```

Adjust `PATH_PREFIX` to match the path your secondary app is served under.

### 6. HTTPS + HTTP redirect frontends

```bash
gcloud compute target-https-proxies create lb-https-proxy \
  --project="${PROJECT_A}" \
  --url-map=lb-url-map --ssl-certificates=lb-ssl-cert --global

gcloud compute forwarding-rules create lb-https-forwarding-rule \
  --project="${PROJECT_A}" --global \
  --load-balancing-scheme=EXTERNAL_MANAGED --network-tier=PREMIUM \
  --address=lb-static-ip --target-https-proxy=lb-https-proxy --ports=443

cat > /tmp/lb-http-redirect.yaml <<EOF
name: lb-http-redirect
defaultUrlRedirect:
  httpsRedirect: true
  redirectResponseCode: MOVED_PERMANENTLY_DEFAULT
EOF

gcloud compute url-maps import lb-http-redirect \
  --project="${PROJECT_A}" --source=/tmp/lb-http-redirect.yaml --global --quiet

gcloud compute target-http-proxies create lb-http-proxy \
  --project="${PROJECT_A}" --url-map=lb-http-redirect --global

gcloud compute forwarding-rules create lb-http-forwarding-rule \
  --project="${PROJECT_A}" --global \
  --load-balancing-scheme=EXTERNAL_MANAGED --network-tier=PREMIUM \
  --address=lb-static-ip --target-http-proxy=lb-http-proxy --ports=80
```

### 7. DNS

1. Open your DNS provider's console for `DOMAIN`.
2. Set A records pointing to the LB static IP (from step 2).
3. **Remove** any old Cloud Run domain mappings or `ghs.googlehosted.com` CNAME records that bypass the load balancer.

| Type | Host | Value |
|------|------|--------|
| A | `@` | LB static IP |
| A | `www` | same LB IP |

4. After DNS points at the LB, delete existing Cloud Run domain mappings (they bypass the LB):

```bash
gcloud beta run domain-mappings list \
  --project="${PROJECT_A}" --region="${REGION_A}"

gcloud beta run domain-mappings delete "${DOMAIN}" \
  --region="${REGION_A}" --project="${PROJECT_A}"
gcloud beta run domain-mappings delete "www.${DOMAIN}" \
  --region="${REGION_A}" --project="${PROJECT_A}"
```

5. Wait for the certificate to become `ACTIVE`:

```bash
gcloud compute ssl-certificates describe lb-ssl-cert \
  --project="${PROJECT_A}" --global \
  --format='value(managed.status,managed.domainStatus)'
```

### 8. Verify

```bash
dig +short "${DOMAIN}" A   # must equal LB IP

curl -sI "https://${DOMAIN}/" | head -n 15
curl -sI "https://${DOMAIN}${PATH_PREFIX}/" | head -n 15
```

---

## Common issues

| Symptom | Cause | Fix |
|---------|--------|-----|
| `gcloud run describe` fails in `PROJECT_A` | Service lives in `PROJECT_B` | Always pass `--project=PROJECT_B` |
| Path returns primary app `404` / wrong redirect | DNS still on Cloud Run / `ghs.googlehosted.com`, not LB | Point A records at LB IP; remove old CNAME |
| Cert stuck `PROVISIONING` | DNS not on LB yet | Fix DNS first |
| Cross-project backend unreachable | Missing IAM binding | Grant `roles/compute.loadBalancerServiceUser` on `PROJECT_B` |

---

## Ops checklist

| Item | Owner project |
|------|----------------|
| LB frontend (IP, SSL, URL map, proxies) | `PROJECT_A` |
| Primary service NEG + backend | `PROJECT_A` |
| Secondary service NEG + backend | `PROJECT_B` |
| DNS A records | Your DNS provider |

---

## Rollback (high level)

```bash
# Stop LB traffic
gcloud compute forwarding-rules delete lb-https-forwarding-rule \
  --global --project="${PROJECT_A}" --quiet
gcloud compute forwarding-rules delete lb-http-forwarding-rule \
  --global --project="${PROJECT_A}" --quiet

# Point DNS back / recreate Cloud Run domain mappings if needed
```

---

## References

- [HTTPS load balancing for Cloud Run (serverless NEG)](https://cloud.google.com/load-balancing/docs/https/setup-global-ext-https-serverless)
- [Cross-project service referencing](https://cloud.google.com/load-balancing/docs/https#cross-project)
- [Serverless NEG concepts](https://cloud.google.com/load-balancing/docs/negs/serverless-neg-concepts)
