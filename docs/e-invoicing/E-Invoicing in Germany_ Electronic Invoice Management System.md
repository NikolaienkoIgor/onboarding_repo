# **E-Invoicing in Germany: Electronic Invoice Management System**

# **1\. Definition and Legal Basis**

**A German e-invoice is a structured electronic invoice that can be processed automatically by software.** The invoice must contain its relevant invoice information in a structured electronic format that enables electronic processing. A conventional paper invoice, scanned invoice, or ordinary PDF sent by email is not, by itself, a structured e-invoice.

The German e-invoicing framework is based primarily on **Section 14 of the German VAT Act (Umsatzsteuergesetz, UStG)** and the changes introduced by the **Wachstumschancengesetz (Growth Opportunities Act)**. The framework combines German VAT requirements with the European invoice standard **EN 16931**.

EN 16931 defines the semantic meaning of invoice information. German implementations such as **XRechnung** apply that semantic model to a concrete structured syntax. **ZUGFeRD/Factur-X** combines structured XML with a human-readable PDF/A-3 representation.

A key principle is the distinction between **structured invoice data and its visual representation**. In a hybrid invoice, the structured part is the leading component for the invoice information. If the structured data and the visual representation conflict, the structured data is decisive.

# **2\. Scope of the German E-Invoice Requirement**

**The mandatory German e-invoice rules primarily concern domestic business-to-business (B2B) transactions.** They do not mean that every invoice issued in Germany must always be an e-invoice.

Certain transactions and invoice types are subject to exceptions or separate rules. These can include particular B2C transactions, certain tax-exempt supplies, small-value invoices, passenger tickets, and other legally defined special casesThe public sector requires additional consideration. Business-to-government (B2G) invoicing can be subject to separate federal or state requirements, including specific routing information, electronic submission platforms, and technical specifications. A Leitweg-ID is particularly relevant for routing invoices to public-sector recipients and should not be treated as a universally required field for ordinary B2B invoices.

The e-invoice obligation must therefore always be evaluated using four questions:

1. Is the transaction within the German VAT invoicing rules?  
2. Is the transaction domestic B2B, B2G, B2C, or another category?  
3. Does a statutory exception apply?  
4. Which invoice format and transmission requirements apply to the recipient?

## **3\. Implementation Timeline**

Germany introduced mandatory e-invoicing through a phased implementation rather than a single effective date.

| Date | Requirement |
| ----- | ----- |
| 1 January 2025 | Businesses within scope must be able to receive e-invoices for domestic B2B transactions. |
| 2025–2026 | Transitional rules allow qualifying businesses to continue issuing certain non-structured invoices. |
| 1 January 2027 | Businesses whose previous-year turnover exceeds €800,000 must generally issue e-invoices for covered domestic B2B transactions, subject to the statutory transition and exceptions. |
| 1 January 2028 | The e-invoice issuance obligation extends more broadly to covered domestic B2B transactions, including businesses below the €800,000 threshold, subject to statutory exceptions. The remaining transitional relief expires. |
| Early 2029 | Planned voluntary pilot of the digital VAT reporting system (BMF target). |
| 1 July 2030 | Planned launch of the digital VAT reporting system, coinciding with the EU ViDA digital reporting requirements (BMF target). |

The €800,000 threshold relates to the issuer's previous-year turnover. It does not determine whether a business must be able to receive an e-invoice. The receiving obligation has applied since 1 January 2025\.

The transition period should not be confused with the permanent technical standard. A business may temporarily issue a permitted "other invoice," but this does not make the PDF or paper format an e-invoice.

Germany's current framework is primarily an electronic invoicing and exchange framework. It should be distinguished from future transaction-based electronic VAT reporting requirements, which are described in section 3a.

## **3a. Outlook: From E-Invoicing to E-Reporting**

On 22 September 2026, at an event of the German Electronic Invoice Association (VeR), Michael Schrodi, Parliamentary State Secretary at the Federal Ministry of Finance (BMF), confirmed the target timetable for digital VAT reporting: a **voluntary pilot from the beginning of 2029** and a **planned go-live on 1 July 2030**.

The BMF describes the reform as a staged move from conventional accounting, through structured electronic invoices, towards the digital reporting of specified data to the tax administration:

**Structured e-invoice → invoice data → digital VAT reporting**

Structured e-invoices create standardised transaction data. The next stage is to use that data for digital reporting to the tax authorities.

The 1 July 2030 target coincides with the EU VAT in the Digital Age (ViDA) digital reporting requirements, which introduce harmonised digital reporting for relevant intra-EU B2B transactions from that date. Germany is therefore developing its domestic reporting system against the same timetable, which may allow the data and technology behind domestic reporting to be aligned with the EU framework. The detailed German requirements remain to be determined.

The BMF also sees a potential role for commercial IT providers and e-invoicing platforms, both in invoice transmission and, subsequently, in reporting invoice data to the tax administration.

**The technical architecture has not yet been confirmed.** There is currently no BMF confirmation that Germany will mandate Peppol, adopt a particular four- or five-corner model, or require separate buyer-side reporting. VeR has proposed models using existing e-invoicing infrastructure, including Peppol and a five-corner model that includes the tax authority, but these remain industry proposals rather than the final German reporting design.

For businesses, the practical consequence is that the German e-invoicing mandate should not be treated simply as a change of invoice format. It is the first stage of a wider transition towards transaction-level digital VAT reporting. ERP data, VAT determination, structured invoice data and VAT reporting will increasingly need to reconcile as parts of the same digital compliance process.

*Source for this section: vatcalc.com, "Germany BMF targets July 2030 VAT e-reporting launch", 28 September 2026\. Later developments should be checked against official BMF publications.*

## **4\. EN 16931: The Semantic Foundation**

EN 16931 defines what invoice information means and how the information is logically structured. It is a semantic data model rather than one single file format.

The standard organizes invoice information into concepts such as:

* invoice identifier;  
* invoice date;  
* seller and buyer;  
* delivery information;  
* invoice lines;  
* quantities and units;  
* prices;  
* VAT categories and rates;  
* allowances and charges;  
* payment terms;  
* invoice totals.

The standard uses Business Terms (BTs) to identify individual data concepts. Examples include BT-1 for the invoice number, BT-2 for the invoice issue date, and BT-109 for the invoice total without VAT.

An XML document is not automatically compliant simply because it is XML. The XML must implement an applicable invoice syntax and satisfy the relevant semantic and business rules.

## **5\. Accepted E-Invoice Formats and Technologies**

### **5.1 XRechnung**

XRechnung is the German structured XML implementation of EN 16931 and is particularly important in public-sector invoicing.

XRechnung is designed for machine processing. A human-readable invoice layout is not required for the XML itself because the invoice data is intended to be processed by software.

XRechnung is used extensively for B2G transactions and can also be used for covered B2B transactions.

### **5.2 ZUGFeRD / Factur-X**

ZUGFeRD/Factur-X is a hybrid e-invoice format that combines structured XML with a human-readable PDF/A-3 representation.

The PDF provides a familiar visual representation for people, while the embedded XML provides the structured information required for automated processing.

The important implementation principle is:

**PDF representation \+ structured XML \= one hybrid invoice container.**

The PDF/A-3 container must comply with the applicable ZUGFeRD/Factur-X specification, including the required PDF version (PDF 1.7 or higher, on which PDF/A-3 is based). The existence of a PDF alone does not make the invoice an e-invoice.

For German compliance, businesses should use a ZUGFeRD/Factur-X profile that is aligned with the applicable EN 16931 requirements. Profiles intended only for reduced or non-EN-16931 data sets should not automatically be treated as compliant e-invoices.

### **5.3 Peppol BIS Billing**

Peppol BIS Billing is a structured billing specification designed for interoperable electronic invoice exchange.

Peppol BIS Billing uses structured invoice data based on the EN 16931 model and is transported through the Peppol network.

It is important to distinguish:

* EN 16931 \= semantic invoice standard;  
* Peppol BIS Billing \= structured billing specification;  
* Peppol network \= transmission infrastructure;  
* XRechnung \= German implementation;  
* ZUGFeRD/Factur-X \= hybrid invoice format.

Peppol is therefore a transport and interoperability solution, not simply another name for EN 16931\.

### **5.4 EDI and Other Agreed Formats**

Electronic Data Interchange (EDI) can continue to be relevant where German transitional or statutory rules permit it.

EDI formats such as EDIFACT should therefore not be described as automatically compliant or automatically non-compliant. The legal treatment depends on the format, contractual agreement, applicable transition rules, and whether the required invoice information can be correctly and completely represented.

## **6\. Conditional Tax Scenarios**

Special transaction types require additional information beyond the standard invoice fields.

### **Reverse Charge**

For a reverse-charge transaction, the invoice must contain the applicable VAT classification and the legally required indication that the recipient is liable for the VAT.

The structured invoice should represent the reverse-charge scenario using the applicable EN 16931 VAT category and reason information.

### **VAT Exemption**

For an exempt transaction, the invoice must identify the applicable exemption treatment and provide the required exemption reason or legal reference where applicable.

The system should not simply set VAT to zero without explaining the legal reason for the treatment.

### **Intra-Community Supplies**

Intra-community supplies require additional VAT information, including the relevant party VAT identification information and the applicable exemption or transaction treatment.

### **Self-Billing**

For self-billing arrangements, the invoice must be identified appropriately and contain the required indication that the invoice was issued by the recipient on behalf of the supplier.

The term "Gutschrift" in German VAT law refers to a self-billed invoice and should not be confused with an English-language commercial "credit note."

### **Prepayments and Final Invoices**

A final invoice that takes previous advance payments into account should reference the relevant advance invoices and show the deduction of amounts already paid in a structured and traceable manner.

### **Margin Scheme**

Transactions covered by the margin scheme require the legally prescribed indication of the applicable special treatment, such as the relevant notation for second-hand goods, collectors' items, or travel services.

## **7\. Invoice Creation and ERP Integration**

The most reliable architecture creates the e-invoice from controlled transactional data inside the ERP, accounting, or billing system.

A typical process is:

**Sales order / delivery / service record → ERP master data → invoice calculation → EN 16931 mapping → XML generation → validation → transmission**

The ERP should maintain a controlled mapping between internal data fields and EN 16931 Business Terms.

Examples include:

* customer master data → BT-44 and buyer address fields;  
* supplier master data → BT-27 and seller address fields;  
* invoice sequence → BT-1;  
* invoice date → BT-2;  
* purchase order reference → BT-13 (header level) and BT-132 (line level);  
* delivery records → BT-72 or BT-73/74;  
* quantity and unit → BT-129 and BT-130;  
* unit codes → UN/ECE unit codes, for example "C62" for "piece";  
* price and price breakdown → BT-146 to BT-150 (net price, price discount, gross price, price base quantity, and base quantity unit);  
* VAT configuration → BT-151/152 and VAT breakdown fields;  
* accounting totals → BT-106 through BT-115.

Native structured generation is an implementation best practice. It reduces manual re-keying, prevents inconsistent calculations, and creates a stronger audit trail.

Retroactively editing an invoice with PDF tools after creation should be avoided. Such edits do not change the embedded XML and can create a conflict between the visual and the structured data. Corrections should be made in the source system and should produce a new or corrected structured invoice.

German VAT law should not, however, be misrepresented as a universal prohibition on every type of manual interaction with invoice data. The critical requirement is that the final document qualifies as a valid e-invoice and that its authenticity and integrity are maintained.

## **8\. Technical Validation**

Validation should occur before an invoice enters the downstream accounting and payment process.

A robust validation architecture performs several layers of checks.

### **Structural validation**

The system verifies that the XML conforms to the applicable XML schema and data types.

Empty data elements, such as empty signature or digest-method tags, can cause technical validation to fail. Optional elements should be omitted rather than sent empty.

### **Semantic validation**

The system verifies that the invoice follows EN 16931 concepts and the applicable implementation rules.

### **Schematron validation**

Schematron rules identify logical relationships and business-rule violations that cannot be detected by basic XML schema validation alone. An invoice should pass all applicable Schematron rules without errors.

### **Code-list validation**

The system verifies standardized codes such as:

* currency codes;  
* VAT category codes;  
* unit-of-measure codes;  
* invoice type codes;  
* electronic address schemes.

### **Mathematical validation**

The system verifies that quantities, prices, line totals, VAT calculations and invoice totals are consistent.

### **Business validation**

The receiving organization can additionally verify:

* supplier identity;  
* purchase order references;  
* contract numbers;  
* tax identifiers;  
* duplicate invoices;  
* payment instructions;  
* commercial values.

Automated validation is a strong operational control, but the use of a particular validation software product is not itself a universal statutory prerequisite for VAT recognition.

An invoice can therefore be technically validated and still require manual business review, and a business can have legal obligations even when its internal validation process is incomplete.

## **9\. Transmission and Routing**

German B2B e-invoicing does not require one single universal transmission network.

Depending on the transaction and recipient, structured invoices can be transmitted through mechanisms such as:

* Peppol (access points);  
* electronic portals;  
* EDI connections;  
* APIs;  
* secure file transfer (for example SFTP);  
* agreed electronic mail processes (structured email transfer);  
* other electronic methods permitted by the applicable rules.

Businesses should maintain the infrastructure needed to send and receive structured invoices through the channels their trading partners use.

The important requirement is that the receiving party obtains the structured invoice in a legally and technically usable form.

For B2G, the recipient's platform and routing requirements can be more specific. Public-sector customers can require defined technical formats, identifiers, portals, and submission procedures.

A buyer should therefore publish a clear trading-partner specification that identifies:

* accepted invoice formats;  
* required Business Terms;  
* routing information;  
* purchase-order requirements;  
* transmission channel;  
* attachment rules;  
* validation and rejection rules.

## **10\. Attachments and Supporting Documents**

Mandatory invoice information must be represented in the structured invoice itself.

A business should not move legally required invoice information into an external attachment and assume that the attachment replaces the corresponding structured field.

Supplementary documentation can nevertheless be useful and, where permitted, may include:

* timesheets;  
* delivery records;  
* service reports;  
* calculation details;  
* specifications;  
* contractual references;  
* supporting evidence.

Supplementary documents should be embedded in the structured invoice container (for example as additional supporting documents within the XML). Files sent separately, such as a separate email attachment, are not part of the e-invoice. For public-sector (B2G) transactions, external attachments are generally not accepted, and the receiving platform's rules on file types, sizes and submission methods apply.

The document-management system should clearly distinguish between:

* legally required invoice information, and  
* supplementary supporting documentation.

## **11\. Inbound Invoice Processing**

Receiving an e-invoice should be treated as a controlled business process rather than simply as receiving an email attachment.

A mature accounts-payable workflow can perform the following sequence:

**Receive → identify → validate → match → approve → post → pay → archive**

### **Supplier verification**

The system compares the supplier's identity and tax information with approved master data.

### **Purchase-order matching**

The system compares invoice information with the purchase order, contract, and delivery or service confirmation where applicable.

### **Duplicate detection**

The system checks invoice identifiers, suppliers, amounts, dates, and other metadata to identify duplicate submissions.

### **Fraud detection**

The system looks for indicators of fraud, such as changed bank details, unusual amounts, or suspicious supplier behavior, before booking and payment approval.

### **Tax checks**

The system checks VAT categories, rates, taxable amounts, and special tax scenarios. Checking incoming invoices for technical and tax correctness helps protect the right to deduct input VAT, which depends on receiving an invoice that meets the legal requirements.

### **Workflow approval**

Invoices that require business approval are routed to the responsible department before posting or payment.

### **Exception management and rejection**

Failed invoices should receive a clear reason for rejection, such as:

* invalid XML structure;  
* missing mandatory data;  
* invalid code;  
* incorrect tax calculation;  
* unknown supplier;  
* invalid routing information;  
* duplicate invoice;  
* unmatched purchase order.

A defined rejection protocol should notify the supplier of the specific technical reason so that a corrected invoice can be issued.

These controls should be understood as internal process and risk-management measures, not as a universal list of statutory e-invoicing requirements.

## **12\. Authenticity and Integrity**

An e-invoice must remain authentic, complete, and traceable throughout its lifecycle.

German VAT rules require businesses to ensure:

* authenticity of origin;  
* integrity of content;  
* readability.

A business may use appropriate internal control procedures to establish a reliable audit trail connecting an invoice with the underlying transaction.

A qualified electronic signature is not automatically required for every German e-invoice.

The invoice-management system should therefore record relevant evidence such as:

* source transaction;  
* creation date;  
* invoice identifier;  
* sender and recipient;  
* transmission event;  
* receipt event;  
* validation result;  
* correction history;  
* approval history;  
* archive location.

An invoice correction should create a traceable business event rather than silently modifying the original invoice.

## **13\. Hybrid Invoice Rules**

ZUGFeRD/Factur-X requires special attention because the same business transaction is represented by both structured XML and a visual PDF.

The correct architecture is:

**PDF/A-3 visual representation \+ embedded structured XML**

The PDF allows people to read the invoice easily. The XML allows software to process the invoice.

The structured data is the leading invoice component. If the PDF displays an amount that conflicts with the corresponding structured value, the structured value is decisive for the invoice information. The visual representation should be kept clearly separate from the structured data and should not be treated as an independent source of truth.

The archive should therefore preserve the integrity of the structured invoice data and ensure that the relationship between the XML and the visual representation remains traceable.

## **14\. Archiving and Retention**

German e-invoices must be archived so that their integrity, readability, traceability, and reproducibility are maintained.

The archive should preserve the original electronic invoice data and relevant processing information.

A strong e-invoice archive should provide:

* original structured invoice data;  
* relevant attachments;  
* metadata;  
* transmission records;  
* validation results where maintained;  
* correction records;  
* approval records where applicable;  
* searchable indexing;  
* controlled access;  
* protection against unauthorized alteration or deletion.

For invoices received electronically, the structured electronic information must remain available in an appropriate form. For hybrid invoices, the retention requirements depend on the information contained in the structured and visual components. Where the human-readable part contains additional tax-relevant information, it must also be preserved appropriately.

The general statutory invoice retention period is eight years under §14b UStG. The retention period begins at the end of the calendar year in which the invoice was issued.

Other records may be subject to different or longer retention requirements. The accounting archive should therefore manage invoice retention together with the organization's broader tax and accounting retention policies.

## **15\. GoBD and Verfahrensdokumentation**

The electronic invoice process must be documented in a way that allows an auditor to understand how the system creates, receives, processes, stores, and reproduces invoices.

The Verfahrensdokumentation should describe at least:

* organizational responsibilities;  
* software and system architecture;  
* ERP and accounting interfaces;  
* invoice creation process;  
* invoice receipt process;  
* data mappings;  
* validation procedures and automation logic;  
* approval workflows;  
* exception handling and supplier rejection procedures;  
* correction procedures;  
* archiving procedures;  
* access controls;  
* backup and recovery processes;  
* protection against alteration;  
* document retrieval;  
* system changes and version management.

The documentation should match the actual technical process.

A document that describes one workflow while the production system operates differently creates an audit and control weakness.

Tax auditors assess compliance on the basis of the documented and actually operated procedures, not on unverified third-party or vendor claims. Software certificates or supplier assurances therefore do not replace the business's own documentation of how its validation and processing work.

The Verfahrensdokumentation should therefore be maintained as a living controlled document, with version history and clearly defined ownership.

## **16\. End-to-End E-Invoice Lifecycle**

A complete German e-invoice management system connects invoice creation, structured data, validation, transmission, processing and archiving into one controlled lifecycle.

1. **Create:** The ERP or billing system creates the invoice from the underlying business transaction.  
2. **Map:** Internal fields are mapped to EN 16931 Business Terms.  
3. **Generate:** The system creates XRechnung, ZUGFeRD/Factur-X, Peppol-compatible structured data, or another permitted format.  
4. **Validate:** The invoice passes structural, semantic, code-list and business-rule checks.  
5. **Approve:** Internal approval rules are applied where required.  
6. **Transmit:** The invoice is sent through the agreed electronic channel.  
7. **Receive:** The recipient captures the original structured invoice data.  
8. **Validate:** The recipient performs technical and business checks.  
9. **Match:** The invoice is compared with supplier, purchase-order, delivery, contract, tax, and payment information, including duplicate and fraud checks.  
10. **Post:** The approved invoice is recorded in the accounting system.  
11. **Pay:** The authorized payment process uses the validated invoice information.  
12. **Archive:** The original electronic invoice and relevant evidence are retained in an auditable archive.  
13. **Audit:** The business can reproduce the invoice, demonstrate its origin, explain its processing, and show the relevant documentation.

Looking ahead, the invoice data created and validated in this lifecycle is expected to become the basis for digital VAT reporting (see section 3a).

## **17\. Compliance Model: Legal, Semantic, Technical and Operational Layers**

The German e-invoicing system works best when four different layers are kept separate.

| Layer | Main Question | Examples |
| ----- | ----- | ----- |
| Legal | What must the invoice contain? | §14 UStG, §14b UStG |
| Semantic | What does each invoice data item mean? | EN 16931, Business Terms |
| Technical | How is the information encoded? | XRechnung, UBL, CII, ZUGFeRD XML |
| Operational | How is the invoice processed? | Validation, routing, matching, approval, archiving |

These layers should not be conflated.

For example:

* EN 16931 is not a transmission network.  
* Peppol is not the German VAT law.  
* XRechnung is not the same thing as Peppol.  
* ZUGFeRD is not simply a PDF.  
* Validation software is not itself the legal definition of an e-invoice.  
* A Leitweg-ID is not a universal B2B invoice requirement.

This separation makes the system easier to implement and makes the resulting knowledge base significantly more reliable.

## **18\. Practical Compliance Checklist**

A German e-invoice process should be able to answer "yes" to the following questions:

| Area | Control Question |
| ----- | ----- |
| Scope | Have the transaction type and applicable e-invoice obligation been identified correctly? |
| Format | Does the invoice use a permitted structured format? |
| EN 16931 | Is the invoice semantically aligned with the applicable EN 16931 implementation? |
| Mandatory data | Are all applicable statutory invoice details (§14 UStG) represented in structured data? |
| Parties | Are seller and buyer correctly identified? |
| Routing | Is the invoice addressed to the correct electronic endpoint? |
| Tax | Are VAT categories, rates, amounts, and special tax scenarios correct? |
| Line items | Are quantities, units (UN/ECE codes), prices, discounts, and line amounts consistent? |
| Totals | Do line amounts, VAT amounts, and invoice totals reconcile? |
| Validation | Has the invoice passed the organization's technical and business checks, including Schematron rules and the absence of empty elements? |
| Inbound controls | Are PO matching, supplier verification, duplicate and fraud checks performed before booking and payment? |
| Transmission | Has the invoice been transmitted through the agreed channel? |
| Attachments | Are supplementary files embedded and handled according to the applicable (including B2G) rules? |
| Rejection | Is there a defined rejection protocol that gives suppliers specific technical feedback? |
| Integrity | Can the business demonstrate that the invoice was not silently altered? |
| Archiving | Is the structured invoice preserved and retrievable? |
| Retention | Is the invoice retained for the applicable statutory period? |
| Documentation | Is the process described in current Verfahrensdokumentation? |
| Auditability | Can the organization reproduce the invoice and explain its complete processing history? |
| Outlook | Is the organization preparing for e-reporting (pilot from early 2029, planned launch 1 July 2030)? |

## **19\. Final Implementation Principle**

A compliant German e-invoice system is not simply a PDF generator with an XML export function. It is an integrated electronic document-management process built around structured invoice data.

The core architecture should connect:

**German VAT law → EN 16931 semantics → permitted invoice syntax → ERP data → validation → transmission → accounting workflow → archive → audit documentation → (future) digital VAT reporting**

The most important implementation principle is to distinguish what the law requires, what the standard defines, what the technical format implements, and what the organization's internal process does.

A well-designed system therefore does not treat the e-invoice as a static document. It treats the invoice as structured, traceable business data with a controlled lifecycle from creation to archival, and, from the planned 2030 reporting launch, onward to digital VAT reporting. This approach supports interoperability, automated processing, tax compliance, operational control, and long-term auditability.

