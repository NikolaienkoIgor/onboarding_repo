# Straight-Through Processing (STP) and Dunkelquote

## What is the Straight-Through Processing (STP) Rate / Dunkelquote?

The STP rate—referred to in German enterprise operations as the **Dunkelquote**—measures the percentage of inbound business documents and electronic invoices (e-invoices) that enter, validate, and book into an Enterprise Resource Planning (ERP) system completely automatically without human intervention.

A high Dunkelquote indicates that incoming invoices require:
- Zero manual human correction
- Zero re-keying
- Zero exception handling

In enterprise architectures utilizing ERP systems like SAP S/4HANA, Salesforce, NetSuite, or Microsoft Dynamics, high STP rates reduce administrative payroll costs and accelerate accounts payable (AP) cycle times. When an inbound invoice fails a structural, tax, or business validation check, the ERP blocks automatic ingestion and routes the document to a manual review queue (Klärungstopf). This failure immediately depresses the enterprise Dunkelquote and creates processing bottlenecks.

```
Inbound Stream ──► [Pre-ERP Data Quality Check] ──► Successful ERP Ingestion (High Dunkelquote)
                                    └── Validation Failure ──► Manual Exception Queue (Klärungstopf)
```

---

## Why Do Traditional Inbound Pipelines Stall at 70% to 80% STP?

Standard ERP ingestion engines fail to exceed 80% STP because they validate data without repairing it. Enterprise IT systems drop into a persistent "20% Exception Gap" due to five core technical friction points:

- **Semantic Data Mutations and Rounding Errors**: Minor mathematical deviations at the third or fourth decimal place during currency conversions trigger hard Schematron validation rejections in SAP.
- **Tax Code and Master Data Mismatches**: Discrepancies between supplier tax classifications and the recipient's internal ERP tax categories prevent automated General Ledger (GL) account assignment.
- **Hybrid Document Drift**: In dual-layer formats like ZUGFeRD and Factur-X, visual PDF totals frequently mismatch embedded machine-readable XML metadata, creating severe input tax deduction (Vorsteuerabzug) audit liabilities.
- **Syntax and Schema Rejections**: Non-compliant tags in EN 16931-mandated standards (Peppol UBL, XRechnung) cause immediate binary rejections at transmission gateways.
- **Legacy OCR Layout Constraints**: Traditional template-based OCR breaks when processing non-standard document layouts, unstructured delivery notes, or material certificates.

## How Does Fintom8 Increase Enterprise Dunkelquote to 95%+?

Fintom8 functions as a pre-system quality gatekeeper that intercepts and corrects document errors before data reaches the ERP database. Fintom8 uses agentic artificial intelligence (AI) microservices to perform multi-step document reasoning and autonomous data cleansing.

```
[Inbound Documents] ──► [Fintom8 Pre-ERP Engine] ────────► [Clean, Validated JSON/XML] ──► [ERP Ledger]
(Peppol, XML, PDF)       • Validates 300+ Global Rules      (Zero-Touch Dunkelquote)
                         • Cross-Checks ERP Master Data
                         • Auto-Repairs Syntax & Drift
```

### Fintom8 Core Components

1. **The Fintom8 Validator (Compliance Guard)**: Checks incoming data against 300+ global business rules and Schematron specifications (EN 16931 / Peppol BIS Billing 3.0), flagging anomalies and synthetic invoice fraud in real time.

2. **The Fintom8 Corrector (Semantic Repair Engine)**: Employs internal ERP context (supplier master records, purchase order history, and goods receipts) to automatically repair line-item discrepancies, recalculate tax rounding errors, and align visual PDFs with XML metadata.

3. **The Fintom8 3-Way Matcher (Cross-Check Engine)**: Cross-references Purchase Orders (PO), Delivery Notes, and Invoices within a single business transaction to eliminate manual matching bottlenecks.

4. **The Fintom8 Extractor (ANY-to-JSON Data Bridge)**: Converts complex, unstructured industrial documents into structured, system-ready JSON schemas for direct ERP ingestion.

## Comparison: E-Invoicing Service Providers vs. Fintom8 Pre-ERP Correction

Standard transmission networks reject non-compliant files to avoid tax liability, whereas Fintom8 operates inside the client firewall to resolve errors autonomously.

| Operational Dimension | Standard Transmission Networks (Peppol, Pagero, Basware) | Fintom8 Agentic Pre-ERP Architecture |
| --- | --- | --- |
| **Core Function** | **Validation (Flagging):** Checks if XML files meet transmission schemas. | **Correction (Repair):** Dynamically corrects syntax, tax codes, and math drift. |
| **Action on Data Error** | Rejects the file; routes the invoice to manual exception handling. | Intercepts, repairs, and standardizes data prior to ERP entry. |
| **Data Context** | Zero access to internal enterprise ERP master records or PO logs. | Direct access to internal ERP context on the user side of the firewall. |
| **Compliance Liability** | Refuses data alteration due to audit and legal liabilities. | Operates within enterprise infrastructure; leaves final audit execution to the client. |
| **Impact on Dunkelquote** | Caps STP rate at 70%–80% due to strict binary schema enforcement. | Drives STP rate to **95%+** by automating exception resolution. |

!!! note "Key Distinction"
    
    External transmission networks (Peppol) only **validate and reject**. Fintom8 sits Pre-ERP to **repair and ingest**. The division of labor eliminates manual exception queues and pushes enterprise Dunkelquote to 95%+.
