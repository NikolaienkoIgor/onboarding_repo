# What Is Peppol?

**Peppol** is a **secure international network for exchanging electronic business documents**, especially e-invoices.

Think of it like **a standardized postal system for businesses**:

> Instead of sending an invoice as a PDF by email, a company can send structured invoice data directly from its accounting/ERP system to the customer's system through Peppol.

## How it works

**Company A (ERP)**

↓

**Peppol Access Point**

↓

**Peppol Network**

↓

**Customer's Access Point**

↓

**Company B (ERP)**

Peppol standardizes **how the documents are formatted, addressed, transported, and validated**.

## What Peppol is NOT

Peppol is **not an invoice format itself**.

For example:

- **EN 16931** → defines the semantic requirements for an electronic invoice
- **Peppol BIS Billing 3.0** → defines how an invoice is exchanged within Peppol, based on EN 16931
- **UBL / XML** → provides the actual machine-readable structure
- **Peppol** → provides the network and rules for exchanging the document

## Why it matters for Fintom8

A useful way to explain the relationship is:

**ERP → Fintom8 → Peppol → Customer ERP**

Fintom8 can work **before the invoice enters the Peppol exchange process**, making sure the document is correctly structured, converted, validated, and compliant.

So, very simply:

> **Peppol is the infrastructure that lets businesses exchange standardized electronic documents with each other.**
