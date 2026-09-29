# **E-Invoicing in Germany**

## **1\. What is an e-invoice in Germany?**

An e-invoice in Germany is not simply an invoice that is sent electronically. A real e-invoice contains structured data that software can read, process and exchange automatically, without anyone retyping it.

This is the key difference from a traditional invoice. A paper invoice is not an e-invoice. A scanned invoice is not one either. And a normal PDF sent by email is not automatically one.

The framework rests mainly on Section 14 of the German VAT Act, changed by the Wachstumschancengesetz, together with the European standard EN 16931\. The goal is to move from invoices that are mainly documents for people to invoices that can also be processed as structured business data.

## **2\. Is a PDF an e-invoice?**

A normal PDF is not automatically an e-invoice. It is a visual document made for people to read, while an e-invoice carries structured information that software can process.

There is one important exception. ZUGFeRD, also called Factur-X, combines a readable PDF/A-3 document with structured XML data embedded inside it, and the XML provides the invoice information.

The key principle is that the structured data decides. If the visible PDF and the XML disagree, the XML is decisive. So simply converting an invoice to PDF does not make it compliant. During the transition period a PDF can still be used if the recipient agrees, but that is temporary, not permanent.

## **3\. Who is affected and when?**

The rules mainly concern domestic B2B transactions, and they arrive in steps, not on one date.

Since 1 January 2025, businesses in scope must be able to receive and process e-invoices, whatever their size. Until the end of 2026, they can still issue paper invoices, and other formats such as PDF with the recipient's agreement.

On 1 January 2027, which is only a few months away, businesses with more than €800,000 in previous-year turnover must issue structured e-invoices for covered domestic B2B transactions. Smaller businesses get one more year. From 1 January 2028, structured e-invoicing is mandatory for all covered domestic B2B transactions, subject to statutory exceptions.

One point causes confusion: the €800,000 threshold applies to issuing, never to receiving.

## **4\. What is EN 16931?**

EN 16931 is the European standard that defines what information an electronic invoice must contain and what that information means.

It is not a single file format. It is a semantic data model that defines concepts such as invoice number, invoice date, seller, buyer, invoice lines, quantities, prices, VAT information, payment terms and totals.

Different technical formats then implement it. XRechnung is a German structured XML implementation. ZUGFeRD and Factur-X combine structured XML with a readable PDF. Peppol BIS Billing is a structured billing specification based on the same standard.

So an XML file is not compliant just because it is XML. The data and the applicable business rules must comply with the standard.

## **5\. What information has to be inside an e-invoice?**

An e-invoice needs structured information about the document, the seller, the buyer, the transaction, the payment, the VAT and the individual invoice lines.

Important items include the invoice number, date, type and currency, seller and buyer details and addresses, tax identifiers where applicable, payment information, delivery information, quantities, units, prices, discounts, VAT categories and VAT rates.

The totals also have to be consistent. Line amounts, discounts or charges, taxable amounts, VAT, the invoice total and the amount due all need to reconcile.

Not every EN 16931 Business Term is mandatory for every invoice. Whether a field is mandatory, optional or conditional depends on the syntax, the implementation rules, the transaction type and the business situation.

## **6\. Why do invoice fields and Business Terms matter?**

EN 16931 gives each piece of invoice information a Business Term identifier. For example, BT-1 is the invoice number and BT-2 is the issue date.

These identifiers let different systems understand the same information the same way. Instead of one system calling something "Invoice No." and another reading it differently, the standard gives it one defined meaning.

The same applies to invoice lines. Quantity, unit, price, discounts, VAT category and line amount all need to be structured and consistent. Units, for example, use standard codes, such as C62 for a piece and HUR for an hour, not free text.

This is why e-invoicing is not just about producing an XML file. The underlying business data has to be mapped into the standard correctly.

## **7\. What are XRechnung and ZUGFeRD?**

These are the two formats you will meet most often in Germany.

XRechnung is a structured XML implementation of EN 16931\. It is designed mainly for machine processing, and it is particularly important for public-sector invoicing, though it can also be used in B2B.

ZUGFeRD, also known as Factur-X, is a hybrid. It combines a readable PDF/A-3 with structured XML embedded inside. People can read the familiar invoice, while software processes the XML behind it.

Both solve the same problem in different ways: XRechnung goes straight to structured data, while ZUGFeRD adds a visual layer. For ZUGFeRD, the profile you choose should match EN 16931, because reduced profiles do not automatically qualify.

## **8\. What is Peppol, and is it mandatory?**

Peppol is not the same thing as EN 16931\. EN 16931 defines the meaning and structure of invoice data. Peppol provides a specification and a network for exchanging structured business documents.

Peppol BIS Billing uses invoice data based on EN 16931 and lets businesses exchange structured invoices through the Peppol network.

Germany does not require one single transmission network for B2B e-invoices. Depending on the relationship, invoices can also travel through portals, EDI, APIs, secure file transfer or other permitted electronic methods.

So Peppol is one possible way to send an e-invoice. It is not the German VAT law and not the definition of an e-invoice.

## **9\. Can I still use EDI?**

Yes, with conditions. Until 31 December 2027, invoices may still be sent by EDI even if they do not meet EN 16931, as long as the recipient agrees. The BMF confirmed this in a letter of October 2024\. Unlike the paper and PDF relief, this one has no turnover threshold, so companies of any size can use it.

From 2028, EDI is not banned. It stays allowed if the format lets the receiver correctly and completely extract the information EN 16931 requires. In practice, the mapping must work.

So EDI is not dead, but it is no longer exempt from EN 16931 mapping after 2027\. Companies with established EDIFACT or VDA flows should use the extra year to adapt.

## **10\. Why should the ERP create the e-invoice?**

The safest approach is to generate the e-invoice directly from controlled transaction data in the ERP, accounting or billing system.

The process starts with an order, delivery or service record. The system uses master data, calculates the invoice, maps the information to EN 16931 Business Terms, generates the structured invoice, validates it and sends it.

This reduces manual re-entry and keeps the data consistent. It also avoids a trap with hybrid invoices. If someone edits the PDF afterwards, the XML does not change, and the two can disagree. The XML is decisive, so a changed PDF can leave the invoice showing something different from what the tax authority relies on. The fix is to correct the invoice in the source system and generate a new structured invoice.

## **11\. What happens when an e-invoice is validated?**

Validation checks whether an e-invoice is technically and logically correct before it moves on.

First, is the XML well formed? Second, does it follow the applicable schema? Third, do the business rules pass, usually checked with Schematron? That third step is where many systems fail.

A classic pitfall is leaving empty tags in the file, for example an empty signature or digest element. These can make the whole file fail, so optional elements that are not used should be left out, not sent empty.

The system can also check codes, VAT categories, units and the maths: whether quantities and prices give the right line totals, and whether VAT and the invoice total match. An invoice can look correct to a person and still fail automated processing.

## **12\. Why can a compliant invoice still be rejected?**

Legal compliance and commercial acceptance are not the same thing.

An invoice can meet the legal and technical requirements and still be rejected because the customer needs more for its own processes. A purchase order number or a contract reference, for example, may be needed for automated matching, even where the standard treats it as optional.

So businesses need to understand not only the law but also each trading partner's requirements. Customer-specific rules should be documented in one place and built into the invoice mapping and validation. And when an invoice is rejected, the supplier should receive a clear reason, so it can be corrected and reissued.

## **13\. What about attachments?**

Legally required invoice information must be in the structured invoice itself. A company cannot put required tax information into a separate attachment and assume it replaces the structured field.

Additional documents can still be useful, such as timesheets, delivery records, service reports, calculation details or specifications.

Where permitted, these should be embedded inside the structured invoice container. A separate attachment sent in another email is not automatically part of the e-invoice. Public-sector transactions can have further restrictions on attachments and submission methods.

## **14\. When do special tax cases need extra information?**

Some transactions need details beyond the standard fields.

For reverse charge, the invoice must show that the recipient owes the VAT. For a VAT exemption, you cannot simply enter zero percent, because the invoice needs the legal reason. Intra-community supplies need the relevant VAT IDs and exemption treatment. In self-billing, the invoice must be marked as issued by the recipient on behalf of the supplier, and the German term Gutschrift here means a self-billed invoice, not a credit note. A final invoice after prepayments has to reference the advance invoices and show the amounts already paid.

## **15\. What happens when a company receives an e-invoice?**

Receiving an e-invoice should be a controlled accounts-payable process, not simply an email attachment.

A typical process starts with receiving the invoice and identifying the supplier. The system can then validate it, match it against purchase orders and delivery information, check for duplicates, run tax checks and look for signs of fraud, such as changed bank details. After approval, the invoice is posted, paid and archived.

The tax checks matter for a concrete reason. Your right to deduct input VAT, the Vorsteuerabzug, depends on receiving an invoice that meets the legal requirements. If something is wrong, the system should name the specific problem, such as missing mandatory data, invalid XML structure, an incorrect tax calculation, a duplicate or a missing purchase order match.

## **16\. What does e-invoicing mean for IT and ERP teams?**

For IT and ERP teams, e-invoicing is not just a new PDF format. Structured data has to move reliably through the whole invoice process.

The ERP must supply the right information, map internal fields to EN 16931, generate the structured format, validate the result, send it through the required channel and handle errors and rejections.

That makes data quality the real challenge. Finance, tax, sales, procurement, operations and IT each own part of the invoice data. If nobody owns it end to end, errors appear between systems and departments. Most e-invoicing problems are not software bugs, but broken data ownership. The fix is a defined source of truth for each field and clear ownership across teams.

## **17\. What happens to the invoice after processing?**

An e-invoice has to stay authentic, complete, readable and traceable throughout its lifecycle. A qualified electronic signature is not automatically required, but the business must be able to show where the invoice came from, when it was created, who sent and received it, whether it passed validation and whether it was corrected or approved.

The original structured data must be preserved, together with relevant attachments, metadata, transmission information and correction records. Under Section 14b UStG, the general retention period is eight years, starting at the end of the calendar year in which the invoice was issued.

## **18\. What is a Verfahrensdokumentation, and why does it matter?**

Buying e-invoicing software does not automatically make you compliant. In an audit, tax authorities look at how your process is documented and actually run, not at vendor claims.

The Verfahrensdokumentation should describe who is responsible, which systems are used, how invoices are created, validated, received, matched, approved and archived, how errors and corrections are handled, and how access is controlled.

The key rule is that it must match reality. A document that describes one workflow while the system does something else is itself an audit weakness. Treat it as a living document with version history and a named owner.

## **19\. What is the biggest change in 2027 and 2028?**

The big change is that businesses must now issue structured e-invoices, not just receive them.

From 1 January 2027, businesses above the €800,000 threshold must issue structured e-invoices for covered domestic B2B transactions, subject to exceptions. From 1 January 2028, this applies to everyone in scope.

So invoice creation, ERP data, validation, transmission, customer requirements and archiving all have to work with structured data. The transition is a process change as much as a format change.

## **20\. What comes after e-invoicing?**

E-invoicing is part of a broader move toward digital VAT reporting. Structured e-invoices create standardised transaction data, and that data is expected to become the basis for digital reporting to the tax authorities.

The Federal Ministry of Finance has confirmed its target timetable: a voluntary pilot from early 2029 and a planned launch on 1 July 2030\. That date matches the EU's ViDA digital reporting rules. The detailed German requirements and technical architecture are not yet confirmed.

For businesses, this means e-invoicing is the first stage of a longer digital tax-compliance process. ERP data, VAT logic, structured invoices and future reporting will need to stay consistent with one another.

## **21\. The main idea behind German e-invoicing**

German e-invoicing is about turning an invoice from a static document into structured business data.

The process runs from the underlying transaction through ERP data, EN 16931 mapping, invoice generation, validation, transmission, receipt, matching, accounting, payment and archiving. The legal requirements, the EN 16931 standard, the technical format and the company's internal processes are different layers and should not be confused.

A PDF is not automatically an e-invoice. Peppol is not the legal definition of an e-invoice. XRechnung is not the same as Peppol. ZUGFeRD is not simply a PDF. And validation software does not itself decide whether an invoice is legally an e-invoice.

The central principle is structured, accurate, traceable invoice data across the whole lifecycle.

