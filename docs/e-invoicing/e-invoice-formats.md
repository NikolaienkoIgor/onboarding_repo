# E-Invoice Formats

## Hybrid formats (CII-based)

| Format | Description |
|---|---|
| ZUGFeRD Basic | Minimal hybrid invoice, core EN 16931 data, for small businesses |
| ZUGFeRD Comfort | Richer structured data, for medium businesses |
| ZUGFeRD Extended | Most detailed profile, for large enterprises and cross-border use |
| Factur-X | French-German equivalent of ZUGFeRD |

## Pure XML formats (no PDF)

| Format | Description |
|---|---|
| XRechnung | Germany's XML-only format, mandatory for B2G invoicing, EN 16931-compliant |
| UBL (Universal Business Language) | Structured XML standard used in international B2B/B2G |
| Peppol BIS Billing 3.0 | UBL-based format used across the Peppol network, common in public procurement |

---

## ZUGFeRD vs. Peppol

| | Peppol (UBL) | ZUGFeRD (CII) |
|---|---|---|
| Setup | Requires Access Point and network registration | No setup — just generate a PDF |
| Ease of use | Technical onboarding required | Works like a normal invoice |
| Typical flow | Sent through the Peppol network | Emailed or downloaded as a PDF |
| Best fit | Governments, multinationals, high-volume B2B | Freelancers, SMEs, small suppliers |

Both formats are legally EN 16931-compliant. Peppol suits large-scale, networked B2B operations but requires infrastructure most SMEs don't need. ZUGFeRD requires no new infrastructure and keeps the invoice human-readable, which is why Fintom8's PDF-to-hybrid-invoice converter is built around it — it helps SMEs comply with the 2025/2028 mandates without added complexity.
