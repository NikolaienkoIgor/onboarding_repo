# E-Invoicing in Germany

---

## 12 Core Implementation & Operational Requirements

1. **EN 16931 Standard and Format Compliance**  
Invoices must comply with the European standard EN 16931 using approved structured syntaxes such as XRechnung, Peppol BIS, or hybrid ZUGFeRD/Factur-X. For hybrid formats, the XML must be embedded within a compliant PDF/A-3 container with at least PDF version 1.7.

2. **Native ERP XML Generation**  
ERP and billing systems must generate structured XML directly from source transactional data. Post-creation manual data entry or retroactive editing using PDF tools is strictly prohibited.

3. **Schema and Syntactic Integrity**  
The invoice XML must pass all Schematron validation rules without syntax errors or empty data elements. Files containing empty tags (such as empty signature or digest method elements) will fail technical validation.

4. **Statutory Tax Content Inclusion (§ 14 UStG)**  
All mandatory tax details under § 14 UStG must be contained directly within the structured XML elements. Relying on attachments to convey legally required tax information is not permitted.

5. **Standardized Data and Code Mapping**  
Transactional data must be mapped to standardized semantic fields, including header and line-level purchase order references (BT-13). Quantities and pricing components must use standardized formats, such as UN/ECE unit codes (e.g., "C62" for pieces) and itemized price breakdown fields (BT-146 to BT-150).

6. **Designated Transmission Infrastructure**  
Businesses must maintain infrastructure to exchange structured invoices via designated electronic transmission rails. Permitted methods include Peppol network access points, direct electronic interchange (API/SFTP/EDI), or structured email transfer.

7. **Automated Tax and Legal Validation**  
Inbound processing systems must perform automated technical and legal validations on incoming files to verify schema compliance and statutory tax parameters. Completing these checks is required to safeguard the right to deduct input VAT.

8. **Commercial Matching and Fraud Controls**  
Accounts payable software must perform automated commercial checks before booking and payment approval. This includes automated PO matching, supplier verification, duplicate invoice identification, and fraud detection.

9. **Embedded Attachment Rules**  
Any supplementary documentation (such as timesheets or performance records) must be embedded directly within the structured invoice file container. Sending attachments separately via email is invalid, and external attachments are prohibited for public sector (B2G) transactions.

10. **Separation of Visualization and Legal Data**  
The structured XML data must be treated as the sole legally binding invoice record. Any visual, human-readable representations (like rendered PDFs) must be kept strictly separated from the underlying XML.

11. **Formal Internal Process Documentation (*Verfahrensdokumentation*)**  
All internal validation procedures, automation logic, and processing workflows must be formally recorded in written internal process documentation (*Verfahrensdokumentation*). Tax auditors evaluate compliance based on these documented technical procedures rather than unverified third-party claims.

12. **Trading Partner Specifications and Rejection Protocols**  
Defined rejection protocols must be established to notify suppliers with specific technical feedback when an invoice fails validation. Buyers must also issue clear specifications to trading partners outlining mandatory routing data, accepted formats, and transmission paths.

---

## Mandatory XML Fields & Business Terms (EN 16931 / § 14 UStG)

For an electronic invoice to be legally valid and technically accepted by automated validators, all relevant statutory fields and semantic Business Terms (BTs) must be populated directly within the structured XML file.

### 1. Document Level & Header Metadata
| Business Term (BT) | XML / Semantic Field | Statutory Basis / Standard | Description |
| :--- | :--- | :--- | :--- |
| **BT-1** | Invoice number | § 14 (4) No. 4 UStG | Sequential, unique document identifier. |
| **BT-2** | Invoice issue date | § 14 (4) No. 3 UStG | Date of issuance (format: `YYYY-MM-DD`). |
| **BT-3** | Invoice type code | EN 16931 (UNCL 1001) | Document type (e.g., `380` = Commercial Invoice, `381` = Credit Note, `384` = Corrected Invoice). |
| **BT-5** | Invoice currency code | EN 16931 (ISO 4217) | 3-letter currency code (e.g., `EUR`). |
| **BT-10** | Buyer reference | EN 16931 / B2G Mandate | Buyer reference or routing code (in German public sector: *Leitweg-ID*). |
| **BT-23 / BT-24** | Profile & Specification ID | EN 16931 | Formal standard identifier (e.g., `urn:cen.eu:en16931:2017#compliant#urn:xeinkauf.de:kosit:xrechnung_3.0`). |
| **BT-9 / BT-20** | Payment due date / Terms | § 14 (4) UStG / EN 16931 | Exact due date or detailed payment terms. |

---

### 2. Party Information (Seller & Buyer)
| Business Term (BT) | XML / Semantic Field | Statutory Basis / Standard | Description |
| :--- | :--- | :--- | :--- |
| **BT-27 / BT-44** | Seller & Buyer legal name | § 14 (4) No. 1 UStG | Full, registered commercial names of both entities. |
| **BT-35–40 / BT-50–55** | Seller & Buyer address | § 14 (4) No. 1 UStG | Street, postal code, city, and country code (ISO 3166-1 alpha-2). |
| **BT-31 / BT-32** | Seller Tax ID / VAT ID | § 14 (4) No. 2 UStG | German tax number (*Steuernummer*) or German VAT ID (*USt-IdNr.*). |
| **BT-48** | Buyer VAT ID | § 14 (4) No. 2 UStG | Mandatory for cross-border EU B2B or reverse charge transactions. |
| **BT-34 / BT-49** | Electronic addresses | EN 16931 (EAS scheme) | Electronic transmission endpoints (e.g., Peppol ID scheme or official inbox address). |

---

### 3. Supply Details & Line Items (BG-25)
| Business Term (BT) | XML / Semantic Field | Statutory Basis / Standard | Description |
| :--- | :--- | :--- | :--- |
| **BT-72 / BT-73–74** | Delivery date / Period | § 14 (4) No. 6 UStG | Date of delivery/performance or delivery period range. |
| **BT-126** | Line item identifier | EN 16931 | Sequential line numbering (e.g., `1`, `2`, `3`). |
| **BT-153** | Item name / description | § 14 (4) No. 5 UStG | Clear commercial description of the delivered good or service. |
| **BT-129 / BT-130** | Invoiced quantity & unit code | § 14 (4) No. 5 UStG | Exact quantity and standardized UN/ECE Rec 20/21 code (e.g., `C62` for unit/piece, `HUR` for hour). |
| **BT-146** | Item net price | § 14 (4) No. 7 UStG | Net unit price after discounts. |
| **BT-147 / BT-148** | Unit price discount / Gross price | EN 16931 | Gross unit price prior to discount and line discount amount. |
| **BT-149 / BT-150** | Price base quantity & unit | EN 16931 | Base quantity factor (e.g., price per 100 units). |
| **BT-151 / BT-152** | Line VAT category & rate | § 14 (4) No. 8 UStG | Tax category code (e.g., `S` for standard) and applicable percentage (e.g., `19.00`). |

---

### 4. Tax Breakdown (BG-23) & Monetary Totals (BG-22)
| Business Term (BT) | XML / Semantic Field | Statutory Basis / Standard | Description |
| :--- | :--- | :--- | :--- |
| **BT-116 / BT-117** | Taxable base amount & Tax amount | § 14 (4) No. 7 & 8 UStG | Net taxable subtotal per VAT category and calculated tax amount per category. |
| **BT-118 / BT-119** | VAT category code & VAT rate | § 14 (4) No. 8 UStG | Tax rate classification (e.g., Standard, Reduced, Zero, Exempt, Reverse Charge). |
| **BT-106** | Sum of invoice line net amounts | EN 16931 | Total of all invoice line net amounts before document-level charges/allowances. |
| **BT-107 / BT-108** | Document-level allowances / charges | § 14 (4) No. 7 UStG | Header-level discounts or surcharges affecting the taxable base. |
| **BT-109** | Invoice total amount without VAT | § 14 (4) No. 7 UStG | Net invoice total. |
| **BT-110** | Invoice total VAT amount | § 14 (4) No. 8 UStG | Cumulative tax amount across all categories. |
| **BT-112** | Invoice total amount with VAT | § 14 (4) UStG | Gross invoice amount (Net + Total VAT). |
| **BT-115** | Amount due for payment | EN 16931 | Final payable amount remaining after prepayments or paid sums. |

---

### 5. Conditional Statutory Tax Disclosures (Scenarios)
| Scenario | Statutory Basis | XML Implementation Requirement |
| :--- | :--- | :--- |
| **Reverse Charge** | § 13b UStG / § 14 (4) No. 10 UStG | VAT category code set to `AE`. Mandatory statement *"Steuerschuldnerschaft des Leistungsempfängers"* populated in exemption reason (`BT-120`). |
| **Tax Exemption** | § 4 UStG / Intra-community / Export | VAT category code set to `E` or `G`. Mandatory legal citation or exemption reason text in `BT-120` and/or standard reason code in `BT-121`. |
| **Self-Billing** | § 14 (2) UStG | Must use Invoice Type Code `389` (Self-billed invoice) or declare the term *"Gutschrift"* in `BT-22` / `BT-3`. |
| **Prepayments / Final Bill** | § 14 (5) UStG | Explicit structural reference to prior advance payment invoices (`BT-25`) and deduction of prepaid amounts in `BT-113`. |
| **Margin Scheme** | § 25a UStG | Mandatory indicator and notation (*"Gebrauchtgegenstände/Sonderregelung"*, *"Sammlungsstücke"*, or *"Reiseleistungen"*) in invoice note fields (`BT-22`). |