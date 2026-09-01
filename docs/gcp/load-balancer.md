# Load Balancer Setup

How we serve the invoice extractor at `https://fintom8.com/invoice-extractor/` while the marketing site stays at `https://fintom8.com/`.

---

## Goal

| URL | Backend |
|-----|---------|
| `https://fintom8.com/` | Cloud Run `fintom8-website` |
| `https://fintom8.com/invoice-extractor/` | Cloud Run `invoice-extractor` |

Both apps stay in **separate GCP projects**. A global HTTPS Application Load Balancer in the website project routes by path.

---

## Projects and regions

| Role | Project ID | Region | Cloud Run service |
|------|------------|--------|-------------------|
| Website + LB frontend | `fintom8-public` | `europe-west1` | `fintom8-website` |
| Extractor | `poc-bots-484717` | `europe-west3` | `invoice-extractor` |

Project number for extractor (appears in `*.run.app` URLs): `514020438506`.

---

## Architecture

```
Browser  →  DNS (IONOS)  →  Global static IP (fintom8-public)
                              │
                              ▼
                    Global External HTTPS ALB
                    (SSL terminate + URL map)
                              │
              ┌───────────────┴───────────────┐
              │                               │
   /invoice-extractor*                    /* (default)
              │                               │
              ▼                               ▼
   Backend (cross-project)          Backend (same project)
   poc-bots-484717                  fintom8-public
   invoice-extractor-backend        fintom8-website-backend
              │                               │
              ▼                               ▼
   Serverless NEG                   Serverless NEG
   europe-west3                     europe-west1
              │                               │
              ▼                               ▼
   Cloud Run: invoice-extractor     Cloud Run: fintom8-website
```

### Design choices

| Choice | Why |
|--------|-----|
| Two projects | Keep website and extractor ownership separate |
| Global external ALB | Cross-project backend referencing **without** Shared VPC |
| Serverless NEG | Bridge ALB → Cloud Run (Cloud Run cannot attach to a normal backend alone) |
| **No** `pathPrefixRewrite` | Extractor app serves under `/invoice-extractor` itself |
| App `basePath` | Next.js assets and API calls must use `/invoice-extractor/...` or CSS/JS break |

### Glossary

- **NEG (Network Endpoint Group)** — tells the LB where backends live. A *serverless* NEG points at a Cloud Run service.
- **Backend service** — LB config that holds one or more NEGs.
- **URL map** — path/host routing rules.
- **Cross-project reference** — URL map in project A points at a backend service in project B.

---

## App changes (extractor only)

The website needed **no** routing code changes. Path split happens on the LB.

### What we changed in the extractor repo

1. **`frontend/next.config.js`**
   - `basePath` from `NEXT_PUBLIC_BASE_PATH`
   - `trailingSlash: true` when basePath is set (avoids slash redirect loops)

2. **`frontend/src/utils/api.ts`**
   - Production API URLs prefixed with `NEXT_PUBLIC_BASE_PATH` (e.g. `/invoice-extractor/extraction`)
   - Local Next.js still calls `http://localhost:8900/extraction`

3. **`deployment/Dockerfile`**
   - Build-time: `ENV NEXT_PUBLIC_BASE_PATH=/invoice-extractor`

4. **`backend/main.py`**
   - Mount static UI + API under `/invoice-extractor`
   - Keep unprefixed `/extraction` for local backend
   - `/` redirects to `/invoice-extractor/`

### Why path rewrite on the LB was dropped

Early plan: LB strips `/invoice-extractor` and Cloud Run sees `/`.

That breaks if HTML still references `/invoice-extractor/_next/...` while files live at `/_next/...` (or the reverse). Final approach: **app and Cloud Run both use the prefix**; LB only routes, does not rewrite.

Direct Cloud Run URL:

`https://invoice-extractor-514020438506.europe-west3.run.app/invoice-extractor/`

---

## Resource inventory (names we created)

### In `fintom8-public`

| Resource | Name |
|----------|------|
| Global static IP | `fintom8-static-ip` (example value seen: `136.68.200.159`) |
| Serverless NEG | `fintom8-website-neg` (`europe-west1`) |
| Backend service | `fintom8-website-backend` |
| Managed SSL cert | `fintom8-ssl-cert` (`fintom8.com`, `www.fintom8.com`) |
| URL map (HTTPS) | `fintom8-url-map` |
| URL map (HTTP redirect) | `fintom8-http-redirect` |
| Target HTTPS proxy | `fintom8-https-proxy` |
| Target HTTP proxy | `fintom8-http-proxy` |
| Forwarding rule :443 | `fintom8-https-forwarding-rule` |
| Forwarding rule :80 | `fintom8-http-forwarding-rule` |

### In `poc-bots-484717`

| Resource | Name |
|----------|------|
| Serverless NEG | `invoice-extractor-neg` (`europe-west3`) |
| Backend service | `invoice-extractor-backend` |
| Cloud Run | `invoice-extractor` |

### IAM

- On `poc-bots-484717`: `roles/compute.loadBalancerServiceUser` for the user/SA that edits the URL map in `fintom8-public` (e.g. `user:bansal@fintom8.com`).

---

## Setup procedure (commands)

Set variables once:

```bash
gcloud auth login

export WEBSITE_PROJECT=fintom8-public
export EXTRACTOR_PROJECT=poc-bots-484717
export WEBSITE_REGION=europe-west1
export EXTRACTOR_REGION=europe-west3
export WEBSITE_SERVICE=fintom8-website
export EXTRACTOR_SERVICE=invoice-extractor
```

### 0. Prerequisites

```bash
for P in "${WEBSITE_PROJECT}" "${EXTRACTOR_PROJECT}"; do
  gcloud services enable compute.googleapis.com run.googleapis.com --project="${P}"
done
gcloud services enable certificatemanager.googleapis.com --project="${WEBSITE_PROJECT}"

# Redeploy extractor with basePath image
gcloud config set project "${EXTRACTOR_PROJECT}"
./deployment/deploy.sh
```

### 1. Cross-project IAM

```bash
gcloud projects add-iam-policy-binding "${EXTRACTOR_PROJECT}" \
  --member="user:bansal@fintom8.com" \
  --role="roles/compute.loadBalancerServiceUser"
```

### 2. Global static IP (`fintom8-public`)

```bash
gcloud compute addresses create fintom8-static-ip \
  --project="${WEBSITE_PROJECT}" --global --ip-version=IPV4

gcloud compute addresses describe fintom8-static-ip \
  --project="${WEBSITE_PROJECT}" --global --format='value(address)'
```

### 3A. Website NEG + backend (`fintom8-public`)

```bash
gcloud compute network-endpoint-groups create fintom8-website-neg \
  --project="${WEBSITE_PROJECT}" --region="${WEBSITE_REGION}" \
  --network-endpoint-type=serverless --cloud-run-service="${WEBSITE_SERVICE}"

gcloud compute backend-services create fintom8-website-backend \
  --project="${WEBSITE_PROJECT}" --global --load-balancing-scheme=EXTERNAL_MANAGED

gcloud compute backend-services add-backend fintom8-website-backend \
  --project="${WEBSITE_PROJECT}" --global \
  --network-endpoint-group=fintom8-website-neg \
  --network-endpoint-group-region="${WEBSITE_REGION}"
```

### 3B. Extractor NEG + backend (`poc-bots-484717`)

NEG + backend **must** live in the same project as Cloud Run.

```bash
gcloud compute network-endpoint-groups create invoice-extractor-neg \
  --project="${EXTRACTOR_PROJECT}" --region="${EXTRACTOR_REGION}" \
  --network-endpoint-type=serverless --cloud-run-service="${EXTRACTOR_SERVICE}"

gcloud compute backend-services create invoice-extractor-backend \
  --project="${EXTRACTOR_PROJECT}" --global --load-balancing-scheme=EXTERNAL_MANAGED

gcloud compute backend-services add-backend invoice-extractor-backend \
  --project="${EXTRACTOR_PROJECT}" --global \
  --network-endpoint-group=invoice-extractor-neg \
  --network-endpoint-group-region="${EXTRACTOR_REGION}"
```

### 4. Managed SSL (`fintom8-public`)

```bash
gcloud compute ssl-certificates create fintom8-ssl-cert \
  --project="${WEBSITE_PROJECT}" \
  --domains=fintom8.com,www.fintom8.com --global
```

Certificate stays `PROVISIONING` until DNS points at the LB IP.

### 5. URL map (cross-project, no rewrite)

```bash
cat > /tmp/fintom8-url-map.yaml <<EOF
name: fintom8-url-map
defaultService: https://www.googleapis.com/compute/v1/projects/${WEBSITE_PROJECT}/global/backendServices/fintom8-website-backend
hostRules:
- hosts:
  - fintom8.com
  - www.fintom8.com
  pathMatcher: fintom8-paths
pathMatchers:
- name: fintom8-paths
  defaultService: https://www.googleapis.com/compute/v1/projects/${WEBSITE_PROJECT}/global/backendServices/fintom8-website-backend
  routeRules:
  - priority: 1
    matchRules:
    - prefixMatch: /invoice-extractor
    routeAction:
      weightedBackendServices:
      - backendService: https://www.googleapis.com/compute/v1/projects/${EXTRACTOR_PROJECT}/global/backendServices/invoice-extractor-backend
        weight: 100
EOF

gcloud compute url-maps import fintom8-url-map \
  --project="${WEBSITE_PROJECT}" --source=/tmp/fintom8-url-map.yaml --global --quiet
```

### 6. HTTPS + HTTP redirect frontends

```bash
gcloud compute target-https-proxies create fintom8-https-proxy \
  --project="${WEBSITE_PROJECT}" \
  --url-map=fintom8-url-map --ssl-certificates=fintom8-ssl-cert --global

gcloud compute forwarding-rules create fintom8-https-forwarding-rule \
  --project="${WEBSITE_PROJECT}" --global \
  --load-balancing-scheme=EXTERNAL_MANAGED --network-tier=PREMIUM \
  --address=fintom8-static-ip --target-https-proxy=fintom8-https-proxy --ports=443

cat > /tmp/fintom8-http-redirect.yaml <<EOF
name: fintom8-http-redirect
defaultUrlRedirect:
  httpsRedirect: true
  redirectResponseCode: MOVED_PERMANENTLY_DEFAULT
EOF

gcloud compute url-maps import fintom8-http-redirect \
  --project="${WEBSITE_PROJECT}" --source=/tmp/fintom8-http-redirect.yaml --global --quiet

gcloud compute target-http-proxies create fintom8-http-proxy \
  --project="${WEBSITE_PROJECT}" --url-map=fintom8-http-redirect --global

gcloud compute forwarding-rules create fintom8-http-forwarding-rule \
  --project="${WEBSITE_PROJECT}" --global \
  --load-balancing-scheme=EXTERNAL_MANAGED --network-tier=PREMIUM \
  --address=fintom8-static-ip --target-http-proxy=fintom8-http-proxy --ports=80
```

### 7. DNS (IONOS)

Registrar / DNS host: **IONOS** (`ns*.ui-dns.*`).

1. Open [IONOS Domains](https://my.ionos.com) → `fintom8.com` → DNS.
2. Set A records to the LB IP (from step 2).
3. **Remove** old Cloud Run / Google Hosted records:
   - `www` CNAME → `ghs.googlehosted.com`
   - Apex A records → `216.239.x.x`

| Type | Host | Value |
|------|------|--------|
| A | `@` | LB static IP (e.g. `136.68.200.159`) |
| A | `www` | same LB IP |

4. After DNS points at the LB, delete Cloud Run domain mappings (they bypass the LB):

```bash
gcloud beta run domain-mappings list \
  --project="${WEBSITE_PROJECT}" --region="${WEBSITE_REGION}"

gcloud beta run domain-mappings delete fintom8.com \
  --region="${WEBSITE_REGION}" --project="${WEBSITE_PROJECT}"
gcloud beta run domain-mappings delete www.fintom8.com \
  --region="${WEBSITE_REGION}" --project="${WEBSITE_PROJECT}"
```

5. Wait for cert `ACTIVE`:

```bash
gcloud compute ssl-certificates describe fintom8-ssl-cert \
  --project="${WEBSITE_PROJECT}" --global \
  --format='value(managed.status,managed.domainStatus)'
```

### 8. Verify

```bash
dig +short fintom8.com A   # must equal LB IP, not 216.239.*

curl -sI "https://fintom8.com/" | head -n 15
curl -sI "https://fintom8.com/invoice-extractor/" | head -n 15
```

Browser: open `https://fintom8.com/invoice-extractor/` — `/invoice-extractor/_next/...` and `/invoice-extractor/extraction` should be 200.

---

## Issues we hit (and fixes)

| Symptom | Cause | Fix |
|---------|--------|-----|
| Unstyled UI on `*.run.app/` | `basePath` set; assets requested under `/invoice-extractor/_next` while app mounted at `/` | Serve FastAPI static + API under `/invoice-extractor`; redirect `/` → `/invoice-extractor/` |
| `gcloud run describe invoice-extractor` fails in `fintom8-public` | Service lives in `poc-bots-484717` | Always pass `--project=poc-bots-484717` |
| `/invoice-extractor/` returns website `308` / Next.js `404` | DNS still on Cloud Run / `ghs.googlehosted.com`, not LB | Point IONOS A records at LB IP; remove old CNAME |
| Cert stuck `PROVISIONING` | DNS not on LB yet | Fix DNS first |
| Azure/DHL `ConnectTimeout` after VPC egress | `all-traffic` without Cloud NAT, or NAT IP not allowlisted | See [Cloud NAT (egress)](#optional-cloud-nat-static-outbound-ip) |
| `template.vpcAccess: null` after VPC update | Normal for **Direct VPC** | Check annotations `run.googleapis.com/network-interfaces` and `vpc-access-egress` |

---

## Optional: Cloud NAT (static outbound IP)

**Not part of the load balancer.** Use this when partners (e.g. DHL sandbox) need a **fixed allowlisted egress IP**.

Inbound (`fintom8.com` → Cloud Run) ≠ outbound (Cloud Run → Azure/DHL).

### Resources (`poc-bots-484717`, `europe-west3`)

| Resource | Name | Notes |
|----------|------|--------|
| Regional static IP | `fintom8-nat-ip` | Example: `34.185.155.121` — give this to partners |
| Cloud Router | `fintom8-router` | |
| Cloud NAT | `fintom8-nat` | Uses `fintom8-nat-ip` |

### Commands

```bash
export PROJECT_ID=poc-bots-484717
export REGION=europe-west3
export PROJECT_NUMBER=$(gcloud projects describe "${PROJECT_ID}" --format='value(projectNumber)')

gcloud compute addresses create fintom8-nat-ip \
  --project="${PROJECT_ID}" --region="${REGION}"

gcloud compute routers create fintom8-router \
  --project="${PROJECT_ID}" --network=default --region="${REGION}"

gcloud compute routers nats create fintom8-nat \
  --project="${PROJECT_ID}" --router=fintom8-router --region="${REGION}" \
  --nat-external-ip-pool=fintom8-nat-ip --nat-all-subnet-ip-ranges

# Needed if default LLM is Vertex (Google APIs) with all-traffic
gcloud compute networks subnets update default \
  --project="${PROJECT_ID}" --region="${REGION}" \
  --enable-private-ip-google-access

gcloud projects add-iam-policy-binding "${PROJECT_ID}" \
  --member="serviceAccount:service-${PROJECT_NUMBER}@serverless-robot-prod.iam.gserviceaccount.com" \
  --role="roles/compute.networkUser"

gcloud run services update invoice-extractor \
  --project="${PROJECT_ID}" --region="${REGION}" \
  --network=default --subnet=default --vpc-egress=all-traffic
```

Verify Direct VPC (do **not** rely on `template.vpcAccess`):

```bash
gcloud run services describe invoice-extractor \
  --project=poc-bots-484717 --region=europe-west3 \
  --format='yaml(spec.template.metadata.annotations)'
# Expect:
#   run.googleapis.com/network-interfaces: ...default...
#   run.googleapis.com/vpc-access-egress: all-traffic
```

**Private Google Access** is only for Google APIs (Vertex). Azure/DHL only need NAT + allowlist of `fintom8-nat-ip`.

---

## Ops checklist

| Item | Owner project |
|------|----------------|
| Redeploy extractor after basePath / Dockerfile changes | `poc-bots-484717` |
| Website redeploy for path routing | Not required |
| LB frontend (IP, SSL, URL map, proxies) | `fintom8-public` |
| Extractor NEG + backend | `poc-bots-484717` |
| DNS A records | IONOS |
| Static egress for partner APIs | NAT in `poc-bots-484717` |

---

## Rollback (high level)

```bash
# Stop LB traffic
gcloud compute forwarding-rules delete fintom8-https-forwarding-rule \
  --global --project=fintom8-public --quiet
gcloud compute forwarding-rules delete fintom8-http-forwarding-rule \
  --global --project=fintom8-public --quiet

# Point DNS back / recreate Cloud Run domain mappings if needed
```

---

## References

- [HTTPS load balancing for Cloud Run (serverless NEG)](https://cloud.google.com/load-balancing/docs/https/setup-global-ext-https-serverless)
- [Cross-project service referencing](https://cloud.google.com/load-balancing/docs/https#cross-project)
- [Serverless NEG concepts](https://cloud.google.com/load-balancing/docs/negs/serverless-neg-concepts)
- [Static outbound IP (Cloud Run + NAT)](https://cloud.google.com/run/docs/configuring/static-outbound-ip)
