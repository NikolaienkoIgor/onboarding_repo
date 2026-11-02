# France

## Technical Report: France’s E-Invoicing Reform

### 1. Network Architecture: The French "Y" Model (Schéma en Y)

Unlike single-network implementations (such as Peppol-only deployments in Norway or Singapore), France implements a hybrid "Y"-shaped architectural model combining centralized state oversight with decentralized platform exchange.

              [ PPF / Tax Authority ]
         (Central Directory & e-Reporting)
                    /          \
                   /            \
         [ Sender PDP ] ----> [ Receiver PDP ]
               |                      |
           [ Sender ]             [ Receiver ]

- Centralized Core (The Annuaire & PPF): A central public registry (Annuaire) operated by the Portail Public de Facturation (PPF) maps all French business identifiers (SIREN/SIRET) to their designated routing platforms.
- Decentralized Exchange (PDPs): Invoicing operations follow a 4-corner model. Businesses route invoices through private accredited platforms (Partenaires de Dématérialisation Partenaires — PDP) or the public portal (PPF).
- Interoperability with Peppol: While interoperable with Peppol standards, France does not use Peppol as the primary network. The French framework enforces sovereign identification rules (SIREN/SIRET) and domestic routing semantics.

### 2. Key Technical & Operational Challenges

#### A. Routing and Directory (Annuaire) Failures

- SIREN vs. SIRET Confusion: Misalignments between top-level legal entity IDs (SIREN) and site-specific branch IDs (SIRET) cause routing breakdowns within the central Annuaire.
- Explicit vs. Silent Transmission Failures:
  - Explicit Failures (DEST_INC, ADR_ERR): Immediate transmission rejections triggered when a destination identifier does not exist or an address is malformed.
  - Silent Rejections (REJ_ADR): Occurs when a routing address exists in the Annuaire but is inactive or misconfigured. The invoice registers as successfully transmitted on the sender’s side while disappearing into an unmonitored "ghost inbox."
- Enterprise Mapping Complexities: Routing failures frequently stem from unresolved internal enterprise hierarchies and Master Data Management (MDM) gaps exposed by automated validation.

#### B. Strict Rule & Syntax Enforcements

- CIUS-FR Validation Rejections: Rigid enforcement of French domestic business rules (Core Invoice Usage Specification) causes systemic rejections at platform entry points.
- UN/ECE Rec 20 Unit Code Conflicts:
  - Legacy ERPs often export non-standard Unit of Measure (UoM) codes (e.g., PC for Piece, EA for Each).
  - European standards mandate UN/ECE Recommendation 20 standard codes (e.g., H87 for Piece).
  - Failure to map legacy codes triggers hard validation errors (e.g., rule BR-CL-23), rejecting invoice line items.

#### C. Factur-X & Hybrid Discrepancies

- Calculation Drift Between Dual Pipelines: Generating hybrid Factur-X files via separate rendering engines leads to data divergence:
  - Visual Path (PDF/A-3): Computes and rounds amounts at the line level.
  - Structured Data Path (XML/CII): Aggregates raw, unrounded unit values and applies rounding at the document total.
- The Result: Legal discrepancies arise between the human-readable PDF and the legally binding machine-readable XML payload.

### 3. Core Architectural & Strategic Insights

- Single-Source-of-Truth Architecture: Factur-X reconciliation issues are architectural flaws, not format limitations. Systems should generate the human-readable PDF directly from the validated XML payload rather than running parallel pipelines.
- Legal Primacy under EN 16931 & ViDA: Under European norm EN 16931 and upcoming EU ViDA (VAT in the Digital Age) regulations, the structured XML payload (CII or UBL) is the sole legally binding invoice; the visual PDF layer is auxiliary.
- Operational Readiness vs. Technical Connectivity: Connecting to a PDP or the PPF solves only basic gateway transport. Sustainable compliance requires end-to-end exception workflows, lifecycle status management, and automated master data remediation.

### 4. Technical Terminology & Reference Guide

| Term | Category | Definition & Context |
| :--- | :--- | :--- |
| **PPF** | Infrastructure | *Portail Public de Facturation*. The French government central portal managing the central directory (*Annuaire*) and basic exchange/e-reporting services. |
| **PDP** | Infrastructure | *Partenaire de Dématérialisation Partenaire*. State-accredited private service platform authorized to validate, transform, and route e-invoices directly between trading partners. |
| **Annuaire** | Directory | Centralized directory managed by the PPF that maps French company identifiers (SIREN/SIRET) to designated routing platforms (PDP/PPF). |
| **SIREN** | Identifier | 9-digit French identification number representing a legal entity (company level). |
| **SIRET** | Identifier | 14-digit French identification number (9-digit SIREN + 5-digit NIC code) representing a specific physical establishment or branch. |
| **TIN / GLN** | Identifier | **TIN:** Tax Identification Number.<br>**GLN:** Global Location Number (GS1 standard). International routing identifiers referenced in Peppol networks. |
| **SAP** | ERP System | Enterprise Resource Planning platform whose legacy configuration defaults often require updates to align with e-invoicing standards. |
| **KBA** | Documentation | *Knowledge Base Article*. Vendor-published technical note detailing workarounds, patches, or configuration changes (e.g., UoM mapping). |
| **DEST_INC** | Status Code | *Destinataire Inconnu* (Recipient Unknown). Error indicating the recipient identifier does not exist in the central directory. |
| **ADR_ERR** | Status Code | *Erreur d'Adresse* (Address Error). Error indicating the recipient routing address is syntactically invalid or unresolvable. |
| **REJ_ADR** | Status Code | *Adresse Rejetée* (Address Rejected). Status indicating the destination exists in the registry but is inactive or ineligible to receive invoices. |
| **CIUS-FR** | Standard | *Core Invoice Usage Specification (France)*. National implementation of European standard EN 16931, adding mandatory French tax and business rules. |
| **UN/ECE Rec 20** | Standard | UN Economic Commission for Europe Recommendation No. 20. The standard international code list for Units of Measure (UoM). |
| **H87** | Data Code | UN/ECE Rec 20 standard code representing "Piece" (individual item), replacing legacy ERP codes like `PC` or `EA`. |
| **BR-CL-23** | Business Rule | EN 16931 validation rule requiring invoice line Unit of Measure codes to strictly conform to UN/ECE Recommendation 20. |
| **Factur-X** | Format | Franco-German hybrid invoice format combining a human-readable PDF/A-3 file with an embedded machine-readable XML payload. |
| **CII** | Syntax | *Cross Industry Invoice*. An XML schema standardized by UN/CEFACT, used as the structured payload in Factur-X. |
| **ViDA** | EU Directive | *VAT in the Digital Age*. EU legislative initiative harmonizing digital VAT reporting and standardizing cross-border e-invoicing. |

---

Hybrid e-invoices such as Factur-X and ZUGFeRD combine two formats in one document:

- PDF for human-readable viewing
- XML for automated processing by ERP and accounting systems

## How it works

ERP / Sender → Sender Access Point → Receiver Access Point → Buyer ERP

### 1. ERP / Sender

The sender creates a Factur-X PDF containing embedded CII XML. The XML contains structured invoice data such as invoice number, date, supplier, buyer, tax and totals.

### 2. Sender Access Point

The Access Point receives the Factur-X document and prepares it for Peppol. It can extract the embedded CII XML and keep the original PDF as a visual attachment.

The invoice data is then validated against the relevant Peppol and EN 16931 requirements.

### 3. Peppol Network

The structured invoice is sent from one Access Point to another using the AS4 protocol.

AS4 provides secure transmission through encryption, digital signatures and authentication.

### 4. Buyer / Receiver

The receiver's Access Point validates and delivers the invoice to the buyer's ERP or accounting system.

The XML is processed automatically, while the PDF can be used for human viewing.

---

## Factur-X: Two possible approaches

### A. Extract XML + attach PDF

The Access Point extracts the CII XML from the Factur-X PDF and sends the structured XML together with the visual PDF attachment.

Factur-X PDF → CII XML + PDF → Validation → Peppol → Receiver

### B. Direct CII processing

Because Factur-X uses UN/CEFACT CII, CII can be exchanged directly where the relevant Peppol specification and document identifier are supported.

---

## UBL vs. CII

|  | UBL | CII |
| --- | --- | --- |
| Standard | OASIS UBL | UN/CEFACT |
| Root element | `Invoice` / `CreditNote` | `CrossIndustryInvoice` |
| Main structures | `cac:` / `cbc:` | `rsm:` / `ram:` |
| Common use | Peppol BIS Billing 3.0 | Factur-X / ZUGFeRD |
| Hybrid format | PDF handled separately | PDF + XML embedded |

Factur-X = PDF + CII XML

Peppol BIS Billing 3.0 = primarily UBL-based

---

## What is Base64?

Base64 is an encoding method used to represent binary files, such as PDFs, as text.

It can be used when a PDF needs to be included inside an XML or API message.

PDF → Base64 → XML/API → Transmission → PDF

Base64 is not encryption. Security is provided by mechanisms such as HTTPS, digital signatures and AS4 encryption.

### In simple terms

- XML = machine-readable invoice data
- PDF = human-readable invoice
- Peppol = standardized exchange network
- AS4 = secure message transmission
- Base64 = way to carry binary files inside text-based messages

---

## Important post

[LinkedIn](https://www.linkedin.com/posts/bogdannafornita_after-the-french-champagne-has-sizzled-share-7501641325696409604-CID7/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAATWsfoBlBb8ta0aL18UEQa_GltTO8BF1ow)
