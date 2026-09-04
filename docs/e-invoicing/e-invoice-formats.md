# E-Invoice Formats

## EN 16931: The Standard Behind Every Format

Before looking at the different e-invoice formats, it helps to know the standard they all follow.

**EN 16931** is the European standard that defines what information a valid e-invoice must contain — required fields, how totals must add up, and how VAT must be applied. It was published to implement EU Directive 2014/55/EU, by CEN (the European Committee for Standardization).

!!! info "EN 16931"
    - EN 16931 is not a format itself. It is the shared rulebook that formats are built on top of. ZUGFeRD, Factur-X, XRechnung, UBL, and Peppol BIS are all designed to comply with it — that is what "EN 16931-compliant" means when you see it attached to a format name.
    - **What is VAT?** VAT (Value Added Tax) is a tax added to the price of most goods and services. The seller collects it from the customer at the time of sale and then pays it to the government. It's why the price you pay is usually higher than the seller's base price — that extra amount is the VAT.

A revised version of EN 16931 was approved on October 23, 2025, aligned with the EU's ViDA initiative.

## Pure XML formats (no PDF)

| Format | Description |
|---|---|
| **XRechnung** | Germany's XML-only format, mandatory for B2G invoicing, EN 16931-compliant |
| **UBL** (Universal Business Language) | Structured XML standard used in international B2B/B2G |
| **BIS Billing 3.0** | UBL-based format used across the Peppol network, common in public procurement |

!!! info "Hybrid formats"
    - **Hybrid formats** combine a human-readable PDF invoice with a structured XML file embedded inside it, so the same document can be read by a person and processed automatically by software.
    - **CII-based hybrid formats** — like ZUGFeRD and Factur-X — store their hidden data using **CII (Cross Industry Invoice)**, an XML standard created by UN/CEFACT (a United Nations body that sets rules for international trade documents). In simple terms: the PDF looks like a normal invoice to a human, but inside it there's a CII-formatted XML file that lists the same invoice details (amounts, dates, tax, parties) in a structured way that accounting software can read automatically — without anyone re-typing the data.

## Hybrid formats (CII-based)

| Format | Description |
|---|---|
| **ZUGFeRD Basic** | Minimal hybrid invoice, core EN 16931 data, for small businesses |
| **ZUGFeRD Comfort** | Richer structured data, for medium businesses |
| **ZUGFeRD Extended** | Most detailed profile, for large enterprises and cross-border use |
| **Factur-X** | French-German equivalent of ZUGFeRD |

---

### How an E-Invoice Is Structured

An e-invoice is built in three layers. Each layer answers a different question: **What data is required? How is it structured? And what exact rules apply?**

| Layer | Standard | What it defines | Example | ZUGFeRD |
| --- | --- | --- | --- | --- |
| **1. Semantics** | **EN 16931** | Defines **what invoice information must or can be present** | Invoice number, date, seller, buyer, VAT amount, currency | **ZUGFeRD uses EN 16931 semantic requirements** |
| **2. Syntax** | **UBL** | Defines **how the information is structured in XML** using elements, tags and hierarchy | `<cac:Country>`, `<cbc:ID>`, `<cbc:IssueDate>` | **ZUGFeRD uses XML embedded in a PDF/A-3 file** |
| **3. Profile & Rules** | **Peppol BIS Billing 3.0** | Defines **how UBL must be used for Peppol** — mandatory fields, business rules and allowed code values | `DE` = Germany, `EUR` = Euro | **ZUGFeRD profiles define how the embedded XML is used** |

### In simple terms

**EN 16931 → WHAT**

What information belongs on the invoice?

**UBL → HOW**

How is that information represented in XML?

**Peppol BIS → EXACT RULES**

How exactly must the UBL invoice be structured and which values are allowed?
