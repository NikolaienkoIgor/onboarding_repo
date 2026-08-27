# E-Invoice Formats

## EN 16931: The Standard Behind Every Format

Before looking at the different e-invoice formats, it helps to know the standard they all follow.

**EN 16931** is the European standard that defines what information a valid e-invoice must contain — required fields, how totals must add up, and how VAT must be applied. It was published to implement EU Directive 2014/55/EU, by CEN (the European Committee for Standardization).

EN 16931 is not a format itself. It is the shared rulebook that formats are built on top of. ZUGFeRD, Factur-X, XRechnung, UBL, and Peppol BIS are all designed to comply with it — that is what "EN 16931-compliant" means when you see it attached to a format name.

A revised version of EN 16931 was approved on October 23, 2025, aligned with the EU's ViDA initiative.

## Hybrid formats (CII-based)

| Format | Description |
|---|---|
| **ZUGFeRD Basic** | Minimal hybrid invoice, core EN 16931 data, for small businesses |
| **ZUGFeRD Comfort** | Richer structured data, for medium businesses |
| **ZUGFeRD Extended** | Most detailed profile, for large enterprises and cross-border use |
| **Factur-X** | French-German equivalent of ZUGFeRD |

## Pure XML formats (no PDF)

| Format | Description |
|---|---|
| **XRechnung** | Germany's XML-only format, mandatory for B2G invoicing, EN 16931-compliant |
| **UBL** (Universal Business Language) | Structured XML standard used in international B2B/B2G |
| **Peppol BIS Billing 3.0** | UBL-based format used across the Peppol network, common in public procurement |

---

## ZUGFeRD vs. Peppol

| | Peppol (UBL) | ZUGFeRD (CII) |
|---|---|---|
| Setup | Requires Access Point and network registration | No setup — just generate a PDF |
| Ease of use | Technical onboarding required | Works like a normal invoice |
| Typical flow | Sent through the Peppol network | Emailed or downloaded as a PDF |
| Best fit | Governments, multinationals, high-volume B2B | Freelancers, SMEs, small suppliers |

Both formats are legally EN 16931-compliant. Peppol suits large-scale, networked B2B operations but requires infrastructure most SMEs don't need. ZUGFeRD requires no new infrastructure and keeps the invoice human-readable.

!!! tip "Why Fintom8 uses ZUGFeRD"
    Fintom8's PDF-to-hybrid-invoice converter is built around **ZUGFeRD** — it helps SMEs comply with the 2025/2028 mandates without added complexity.
