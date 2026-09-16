# E-invoice vendor services and pricing

Captured **14 September 2026** from public sites. All amounts **excl. VAT** unless a vendor says otherwise. Prices move; re-check the linked pages before quoting a customer.

These four are not the same product:

| Vendor | Job | Network / mailbox? |
|---|---|---|
| [e-invoice.be](https://e-invoice.be/pricing) | Belgian Peppol Access Point: send/receive UBL, optional PDF→Peppol | Yes (Peppol) |
| [InvoiceXML](https://www.invoicexml.com/pricing) | REST: create / validate / convert / embed / PDF-transform (ZUGFeRD, Factur-X, XRechnung, UBL, CII) | No |
| [InvoiceHub](https://invoicehub.dev/pricing) | REST: validate / generate / convert EN 16931 formats | No |
| [Invoice-Converter.com](https://www.invoice-converter.com/en) | Human review UI: PDF/DOCX → XRechnung, ZUGFeRD, UBL; API on Enterprise | No |

---

## Snapshot (list price)

| | e-invoice.be | InvoiceXML | InvoiceHub | Invoice-Converter |
|---|---|---|---|---|
| **Cheapest paid** | €0.25 / invoice, packages from **€350** | Paddle checkout (EUR not printed on the page) | **€9.25 / mo** (Pro) | **€5** per one-off conversion |
| **Volume** | Enterprise **€0.18** / invoice (from **€1,000**) | Starter 1,800 / yr · Business 12,000 / yr · Pro 60,000 / yr | 2,000 docs / mo (Pro) · 25,000 (Scale **€49 / mo**) | Unlimited web UI on subscriptions |
| **Free** | Account + sandbox + Peppol ID | 30-day trial, no card | Unlimited **validate**; 50 generates / mo, UBL only | Review before paying; 3-day trial on first subscription |
| **PDF → hybrid** | Enterprise: PDF → Peppol e-invoice | `POST /v1/transform/to/zugferd` (experimental AI) + embed/create | Factur-X / ZUGFeRD generate on Pro+ (JSON in, not scan-to-XML as the core story) | PDF → ZUGFeRD 2.5 PDF/A-3 after human review |
| **Peppol send/receive** | Yes, billed per send **and** receive | Create Peppol UBL XML only | UBL generate/validate | UBL file out; you deliver it |

---

## 1. e-invoice.be

**Site:** [e-invoice.be/pricing](https://e-invoice.be/pricing) · [API](https://docs.e-invoice.be/) · [terms](https://e-invoice.be/legal/terms-of-service)

Belgian certified Peppol Access Point. JSON (or PDF on Enterprise) in, UBL BIS 3.0 out, AS4 to the network. Inbox/outbox app, email-to-Peppol, webhooks, MCP.

### Services

- Peppol registration (claim / transfer participant ID)
- Send and receive Peppol invoices
- App + email delivery of inbound invoices
- REST API + webhooks ([OpenAPI](https://api.e-invoice.be/api/openapi.json))
- Email-to-Peppol
- Fraud / phishing protection
- Company / Peppol lookup
- **Enterprise only:** automatic PDF → Peppol e-invoice, multi-team, routing, outbound tracking, analytics, priority support
- Sandbox companies (test mode: send goes to email, not Peppol)

Credits also cover **document conversion** (e.g. PDF → Peppol), not only send/receive ([terms §8.1](https://e-invoice.be/legal/terms-of-service)).

### Pricing

| Plan | List price | Package floor |
|---|---|---|
| Pro | **€0.25 / invoice** | from **€350** |
| Enterprise | **€0.18 / invoice**, volume discounts | from **€1,000** (homepage / terms catalog) |

- No setup fee, no monthly minimum (beyond buying a credit package).
- **Send and receive both consume credits.**
- Credits valid **5 years**, **non-refundable**.
- Extra: Peppol ID transfer out **up to €100**; data export **€100/h** + **€100** PDF dump.
- Belgian 120% tax deduction claimed on packages.
- Prices excl. VAT.

---

## 2. InvoiceXML

**Site:** [invoicexml.com/pricing](https://www.invoicexml.com/pricing) · [API catalogue](https://www.invoicexml.com/api) · OpenAPI `https://api.invoicexml.com/v1/openapi`

Frankfurt-hosted convert/validate/create API. **Not** a Peppol access point. Closest public peer to Fintom8’s generate/validate surface, plus many more format pairs.

Euro amounts on the pricing cards are loaded at runtime from **Paddle** (`live_319a6f36c7affb1936bd01d66bf`). The HTML only shows a spinner + `/mo`. Annual billing is advertised as **~20% off**. Confirm live EUR in checkout.

### Services (REST `https://api.invoicexml.com`)

**Create** (JSON → file)

- `POST /v1/create/zugferd` — ZUGFeRD 2.x hybrid PDF/A-3, all profiles
- `POST /v1/create/facturx`
- `POST /v1/create/xrechnung` — CII or UBL
- `POST /v1/create/ubl` · `/v1/create/cii`

**Validate** (JSON or PDF report; report = one credit)

- ZUGFeRD, Factur-X, XRechnung (KoSIT), UBL, CII  
- `.../report` variants return a PDF

**PDF → e-invoice (AI, marked experimental)**

- `POST /v1/transform/to/{zugferd|facturx|xrechnung|cii|ubl}`

**Syntax convert** (16 directions), including

- UBL ↔ CII, UBL/CII ↔ XRechnung, XRechnung → ZUGFeRD / Factur-X, etc.

**Embed / extract / parse**

- `POST /v1/embed/zugferd` · `/v1/embed/facturx` — your PDF + CII → PDF/A-3
- Extract JSON/XML/attachments; `POST /v1/parse/json` AI parse of a scan

**Render:** XML → visual PDF (XRechnung, UBL, CII)

Also: dashboard upload, Zapier/Make/n8n (Starter+), MCP (Business+), email gateway (Professional).

### Pricing (quotas published; EUR via Paddle)

| Plan | Invoice quota | What you get |
|---|---|---|
| Trial | 30 days, no card | API key |
| **Starter** | **150 / mo** or **1,800 / year** | White-label, Zapier, Make, Salesforce, Power Automate, n8n |
| **Business** | **1,000 / mo** or **12,000 / year** | + MCP, PDF/A-3 wrapping, integration help, 7-day logs |
| **Professional** | **5,000 / mo** or **60,000 / year** | + custom compliance profiles, custom features, priority support, 30-day logs |
| **Enterprise** | **10,000+ / mo** | Custom Schematron, SLA, signed contracts, 12-month logs, live calls, **billing by invoice** |

Zero retention on the hot path (in-memory). Starter/Business/Pro: 30-day trial.

---

## 3. InvoiceHub

**Site:** [invoicehub.dev/pricing](https://invoicehub.dev/pricing) · [docs](https://invoicehub.dev/docs) · API `https://api.invoicehub.dev`

Developer EN 16931 engine: one JSON model → UBL / CII / XRechnung / Factur-X. Validation is the loss-leader.

### Services

- `POST /api/v1/validate` — unlimited on every plan (official CEN Schematron v1.3.16; KoSIT XRechnung 3.0)
- `POST /api/v1/generate` — metered; 422 (fail validation) does **not** count
- Convert between UBL 2.1, CII D16B, XRechnung 3.0 (UBL and CII), Factur-X / ZUGFeRD PDF/A-3 EN 16931 profile
- Receive/parse inbound formats → JSON ([receive](https://invoicehub.dev/receive): every plan can parse every format)
- Hosted MCP at `https://api.invoicehub.dev/mcp`

Free plan: **UBL 2.1 only** for generate. CII, XRechnung, Factur-X need Pro+.

### Pricing

| Plan | Price | Generate quota | Formats |
|---|---|---|---|
| **Free** | **€0** | **50 / month** | UBL 2.1 only |
| **Pro** | **€9.25 / month** or **€92.50 / year** (2 months free) | **2,000 / month** | All current formats |
| **Scale** | **€49 / month** or **€490 / year** | **25,000 / month** | All |
| **Enterprise** | Contact | Custom + SLA | All |

- Validation never meters.
- Quota resets 1st of month UTC. Over quota → HTTP 402.
- Burst cap **600 req/min** on every plan.
- No annual lock-in. Usage metadata logged; invoice bodies not stored as an archive.

---

## 4. Invoice-Converter.com

**Site:** [invoice-converter.com/de](https://www.invoice-converter.com/de) · [EN](https://www.invoice-converter.com/en) · [API](https://www.invoice-converter.com/en/developer-api)

German-hosted **review-then-download** converter. AI extract, you edit fields, then export a validated file. Not a Peppol AP.

Current versions they advertise: **XRechnung 3.0.2**, **ZUGFeRD 2.5 / Factur-X 1.09**.

### Services (web)

- [PDF → XRechnung](https://www.invoice-converter.com/en/pdf-to-xrechnung) (B2G, Leitweg-ID)
- [PDF → ZUGFeRD](https://www.invoice-converter.com/en/pdf-to-zugferd) (hybrid PDF/A-3 + CII)
- [PDF → UBL](https://www.invoice-converter.com/en/pdf-to-ubl) (Peppol-style XML)
- [PDF → Factur-X](https://www.invoice-converter.com/en/pdf-to-facturx) (they do **not** send to Chorus Pro / PDP)
- XRechnung validator
- XRechnung → readable PDF
- Create XRechnung from a form (no PDF)
- Batch up to **10** PDFs (Professional+)
- Inputs: PDF, DOCX, TXT (max **20 MB**). Images/legacy DOC rejected on API.

### API (Enterprise)

[External API V1](https://www.invoice-converter.com/en/developer-api): upload PDF/DOCX/TXT or structured ERP data → async `task_id` → XRechnung, ZUGFeRD, EN 16931, UBL, CII. Hybrid ZUGFeRD PDF needs a PDF source (`format=ZUGFERD`, `download=pdf`).

### Pricing (excl. German VAT)

| Plan | Price | Web UI | Batch | API / email import |
|---|---|---|---|---|
| **One-time** | **€5 / conversion** | 1 invoice | No | No |
| **Starter** | **€10 / mo** (**€120 / year**) | Unlimited | No | No |
| **Professional** | **€15 / mo** (**€180 / year**) | Unlimited | Yes | No |
| **Enterprise** | **€35 / mo** (**€420 / year**) | Unlimited | Yes | Yes: **100 / month** shared Email+API, then **€0.40–0.50** each |

- First subscription: **3-day trial**, cancel unpaid. Returning subscribers billed immediately. Pay-per-use has no trial.
- Money-back if they cannot convert to a supported standard and cannot fix it.
- Retention (privacy FAQ): UI artifacts ~**10 min**; API tasks/results **24 h**; QA raw files **30 days**; metadata up to **365 days**.

Support: Standard (one-time/Starter) → Priority (Professional) → business hours, **no SLA** (Enterprise).

---

## How this sits vs Fintom8

Fintom8’s live `/v1/en16931/generate` and `/compare` are in the **InvoiceXML / InvoiceHub / Invoice-Converter** bucket (format intelligence), not e-invoice.be’s (Peppol mailbox).

Rough unit economics from public list prices (order of magnitude only):

- e-invoice.be Pro: **€0.25** per sent **or** received Peppol document.
- Invoice-Converter web: **€5** per PDF if you never subscribe; API overage about **€0.40–0.50**.
- InvoiceHub Pro: **€9.25 / 2,000** ≈ **€0.005** per successful **generate** if you fill the quota (validate is free).
- InvoiceXML: EUR hidden; compare on **quota** (150–5,000 invoices/month on self-serve) until checkout.

e-invoice.be is expensive per document because you buy **transport + legal Peppol identity**. The other three sell **files**.
