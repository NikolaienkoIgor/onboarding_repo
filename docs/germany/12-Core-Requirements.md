
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