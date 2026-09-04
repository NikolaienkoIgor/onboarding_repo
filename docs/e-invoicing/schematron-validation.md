# **Schematron for E-invoice Validations**

[EN 16931 Schematron rules](https://github.com/ConnectingEurope/eInvoicing-EN16931/tree/master)

[OpenPeppol validation rules](https://github.com/OpenPEPPOL/peppol-bis-invoice-3/tree/master/rules/sch)

[Schematron documentation](https://schematron.com/document/455.html?utm_source=chatgpt.com)

Fintom8 toolkit utilizes **EN 16931 Schematron Validation (CEN/TC 434)** to ensure that every e-invoice is correct.

Schematron validates XML schema, business rules, data consistency, and compliance logic to verify invoice accuracy and compliance, such as:

- All required fields are present
- Totals and line items are consistent
- VAT and tax rules are correctly applied

### **What are XSLT files (the "skeletons")?**

The XSLT files in our repo are compiled Schematron validators. They are not the original Schematron rules, but XSLT 2.0 stylesheets generated from those rules.

Think of it like this:

- Original Schematron rules: human-readable rules (e.g., "Invoice total must equal sum of line items")
- XSLT skeleton/compiled file: machine-executable code that checks those rules

Our repository contains two compiled XSLT files:

- `EN16931-CII-validation-xslt1.xsl` — for CII (Cross Industry Invoice) format
- `EN16931-UBL-validation.xslt` — for UBL (Universal Business Language) format

These files are large (15,000+ lines) because they include:

- The validation rules
- XPath generation logic (to locate errors in XML)
- Error reporting templates
- Namespace handling for different XML formats

**What is SaxonPy/Saxonche?**

SaxonPy is a Python wrapper for the Saxon XSLT processor. It runs XSLT transformations.

**How it works together**

- User submits an XML invoice
- System checks if XML is well-formed
- System detects format (UBL or CII)
- System loads the appropriate XSLT file (pre-compiled)
- SaxonPy applies the XSLT to the XML
- XSLT produces SVRL output (validation report)
- System extracts failed assertions from SVRL
- System returns structured error list

**Recommended monitoring strategy**

- Subscribe to Peppol BIS release notes for regular updates
- Check the CEF GitHub repository periodically for new versions
- Monitor CEN publications for standard revisions

**CEN (European Committee for Standardization)**

- Source: Official EN 16931 standard publisher
- What to monitor: Standard revisions and publications
- Recent update: A revised version of EN 16931 was approved on October 23, 2025, aligning with the EU's "VAT in the Digital Age" (ViDA) initiative

---

# EN16931 Base 1.3.15 vs. OpenPeppol 3.0.20

This report analyzes the synergy between two levels of validation: **Semantic Core (EN16931)** and **Network Extension (Peppol BIS 3.0.20)**. For the Fintom8 platform, this means moving from a simple check of "is this invoice legally valid" to "will this invoice be accepted by the Peppol network and the specific client."

---

## 1. The Overlap: 224 Common Rules (Foundation)

These rules are identical in both specifications. They ensure legal compliance with EU Directive 2014/55/EU.

**Key Categories:**

- **Mathematical Integrity:**
  - `BR-CO-10`: The sum of invoice lines must match the total (`BT-106`).
  - `BR-CO-16`: Amount payable calculation: `Total + Rounding - Paid`.
- **Mandatory Presence:**
  - `BR-01`: Every invoice must have a unique identifier (`BT-1`).
  - `BR-07`: Supplier name (`BT-27`) is mandatory.
- **VAT Rules:**
  - `BR-S-08`: If the standard VAT rate applies, the category code `S` must be specified.
  - `BR-IC-02`: For exports (Intra-community), the reason for tax exemption must be specified.

---

## 2. The "On-Top" Layer: 145 Peppol Rules (v3.0.20)

These are rules **not present** in the base version 1.3.x. Peppol added them to automate processes.

**What Peppol adds on top of the base:**

### A. Technical Validation of Addresses (EAS IDs)

- **Base:** Simply requires "provide some address."
- **Peppol 3.0.20:** Requires "provide ONLY a code registered in Peppol (e.g., 0088 for GLN or 0213 for Ukraine)."
- **Outcome:** Protects against invoice rejection due to outdated registries.

### B. PDF Attachments (The Game Changer)

- **Base:** Conservative about attachments.
- **Peppol 3.0.20:** Has specific rules for `cac:AdditionalDocumentReference`. Binary content (PDFs or images) can now be sent directly in XML.
- **Benefit for Fintom8:** Enables Hybrid Invoices, convenient for both machines and humans.

### C. National Business Rules (Country Specifics)

- **Scandinavia:** Includes the latest requirements for Denmark (DK) and Norway (NO) as of November 2025.

---

## 3. Comparative Summary Table

| Category | EN16931 Base (1.3.x) | Peppol Extension (3.0.20) | Added Value of 3.0.20 |
| --- | --- | --- | --- |
| **Number of Rules** | 224 | 369 (224 + 145) | +65% risk coverage |
| **Scheme Codes (EAS)** | Static / Outdated | Dynamic (Updated 2025) | Current identifiers |
| **PDF Attachments** | Limited / Risky | **Full support** | Fintom8 proprietary "feature" |
| **VAT Validation** | Legal level | Network transmission level | Now "Payment Ready" |
| **National Rules** | None | 40+ country-specific rules | Easy entry to Scandinavian markets |

---

## Conclusion for Fintom8 Strategic Choice

Using **only the 224 base rules** carries the risk of "False Positives."

**Decision:**

We use **3.0.20 as the primary filter**, since it includes all 224 base rules but adds critical protection against technical network errors. The tandem 1.3.15 + 3.0.20 guarantees that a corrected invoice will be paid.
