# Summary & Glossary

!!! abstract "In short"
    An invoice is a bill. An **e-invoice** is the same bill as structured XML data instead of a scanned or printed document, so software can read it directly. Hybrid formats such as **ZUGFeRD** embed that XML inside a normal PDF, so the invoice remains readable by people and processable by machines. Different formats use different XML vocabularies — **CII** (used by ZUGFeRD) or **UBL** (used by Peppol). To confirm an invoice is correct, Fintom8 validates it against **EN 16931** rules using Schematron, compiled into XSLT and executed with SaxonPy, checking against the full 369-rule Peppol set rather than the 224-rule base set alone.

---

## Glossary

| Term | Meaning |
|---|---|
| **EN 16931** | EU standard defining what a valid e-invoice must contain |
| **CEN** | European standards body that publishes EN 16931 |
| **Schematron** | Rule-writing language used to express EN 16931's business rules |
| **XSLT** | Compiled, executable version of the Schematron rules |
| **SaxonPy** | Python engine that runs XSLT against an XML invoice |
| **SVRL** | Validation report produced after running a check |
| **CII** | XML vocabulary used by ZUGFeRD / Factur-X |
| **UBL** | XML vocabulary used by XRechnung and Peppol BIS |
| **ZUGFeRD** | Germany's hybrid PDF+XML e-invoice format |
| **XRechnung** | Germany's XML-only B2G e-invoice format |
| **Peppol BIS Billing 3.0** | International network standard for e-invoicing |
| **B2G / B2B** | Business-to-Government / Business-to-Business |
| **ViDA** | "VAT in the Digital Age" — the EU initiative behind these changes |
