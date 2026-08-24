# Schematron Validation

**EN 16931** is the European standard, published by CEN, that defines what a valid e-invoice must contain — required fields, how totals must add up, how VAT must be applied. A revised version was approved on October 23, 2025, aligned with ViDA.

**Schematron** is the language used to express EN 16931's rules (e.g. "the invoice total must equal the sum of the line items") in a form that can be checked against an XML file.

**XSLT files:** Schematron rules are compiled into XSLT files — large, machine-executable files (15,000+ lines) containing the validation rules, error-location logic, error-reporting templates, and namespace handling. Fintom8 uses two:

- `EN16931-CII-validation-xslt1.xsl` — for CII format
- `EN16931-UBL-validation.xslt` — for UBL format

**SaxonPy** is a Python wrapper for the Saxon XSLT processor. It runs the actual validation.

---

## Validation flow

1. User submits an XML invoice
2. System checks the XML is well-formed
3. System detects the format — UBL or CII
4. System loads the matching XSLT file
5. SaxonPy applies the XSLT to the invoice
6. The XSLT produces an SVRL report (validation results)
7. System extracts failed assertions from the SVRL
8. System returns a structured list of errors

---

## Two levels of validation

| | EN16931 Base (1.3.x) | Peppol Extension (3.0.20) |
|---|---|---|
| Rule count | 224 | 369 (224 + 145) |
| Address codes | Static, can go outdated | Dynamic, network-validated |
| PDF attachments | Limited | Fully supported |
| VAT validation | Legal-level only | Legal + network-transmission level |
| National rules | None | 40+ country-specific rules |

EN16931 Base contains the 224 rules required by EU Directive 2014/55/EU, including:

- `BR-CO-10`: invoice line sum must match the total
- `BR-CO-16`: amount payable = total + rounding − paid
- `BR-01`: every invoice needs a unique ID
- `BR-S-08`: standard VAT rate requires category code `S`

Peppol BIS (3.0.20) includes all 224 base rules plus 145 additional rules for network-level reliability:

- Stricter address codes (EAS IDs) instead of free-text addresses
- Rules for embedding binary content (PDF attachments) inside the XML
- Country-specific rules, e.g. added Denmark/Norway requirements in November 2025

---

## Fintom8's validation strategy

Checking only the 224 base rules can produce a false positive — an invoice that passes but is later rejected for a technical or network reason. Fintom8 validates against the full Peppol 3.0.20 rule set, since it includes the base rules plus the network-level checks needed for an invoice to actually be accepted and paid.

**Staying current:**

- Subscribe to Peppol BIS release notes
- Check the CEF GitHub repository for new XSLT versions
- Monitor CEN publications for EN 16931 revisions
