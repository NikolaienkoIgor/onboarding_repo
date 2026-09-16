# France

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
