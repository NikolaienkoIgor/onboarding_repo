# Common UBL errors

These are the errors that show up most often when a UBL invoice is checked. The code is the rule or identifier that reports the problem.

<style>
.md-typeset table:not([class]) th:first-child {
  min-width: 0;
}
</style>

## Common errors

| No. | Error type name | Error code / identifier | Short explanation |
| --- | --- | --- | --- |
| 1 | **Blank Optional Elements** | `PEPPOL-EN16931-R008` | An optional XML element is present but contains no value. It can be removed if the element is genuinely optional. |
| 2 | **Blank Common Elements** | `PEPPOL-COMMON-R001` | A common XML element is empty. It can be removed only when doing so does not violate mandatory-field requirements. |
| 3 | **Lowercase ISO Country Codes** | `BR-CL-14`, `BR-CL-15` | A country code, such as `de`, is lowercase instead of the required uppercase ISO 3166-1 alpha-2 format (`DE`). |
| 4 | **Date Contains a Midnight Time Component** | XML Schema `xs:date` | A date field contains a time component, such as `2026-10-22T00:00:00`, although the field requires a date-only value. |
| 5 | **Missing Currency Identifier on Item Prices** | UBL currency attribute requirements | An item price is missing its required `currencyID` attribute, even though the applicable currency is unambiguously established. |
| 6 | **Lowercase Unit-of-Measure Codes** | `BR-CL-23` | A quantity's `unitCode` uses an incorrectly cased code, such as `c62` instead of `C62`, and can be normalized using the official code list. |
| 7 | **Accidental Spaces in Standard Codes** | Field-specific validation rule | A recognized code contains unnecessary leading or trailing spaces, causing an exact code-list match to fail. |
| 8 | **Data-URI Prefix in PDF Attachment** | Attachment/Base64 processing | An attachment contains a prefix such as `data:application/pdf;base64,` before the actual Base64 payload. |
| 9 | **Missing PDF MIME-Type Label** | UBL attachment requirements | A PDF attachment lacks the `mimeCode="application/pdf"` attribute needed to identify its media type. |
| 10 | **Unnecessary Schema-Location Hint** | `xsi:schemaLocation` | The XML contains an unnecessary schema-location hint, potentially including a local development path. |
| 11 | **Empty Buyer Reference** *(Missing Data)* | `PEPPOL-EN16931-R008`, `PEPPOL-EN16931-R003` *(BT-10)* | The `<cbc:BuyerReference>` tag is present but contains no value. Either supply the customer's actual routing/reference ID, or remove the tag entirely if an Order Reference (`cac:OrderReference/cbc:ID`) is provided instead. |

## BuyerReference and EndpointID in Peppol BIS Billing 3.0

`BuyerReference` and `EndpointID` are two different fields in a Peppol invoice. Both are important, but they serve different purposes.

- **BuyerReference:** Helps the buyer identify who or which department should handle the invoice internally.
- **EndpointID:** Identifies the electronic address of the party involved in the invoice exchange. The buyer's EndpointID identifies the recipient's electronic address.

Neither field should be treated as a replacement for the other.

### BuyerReference

#### What is it?

BuyerReference is a reference assigned by the buyer to help route the invoice to the correct person, department, or internal process.

For example, it could be a department code, an employee reference, or another identifier provided by the buyer.

#### Who provides it?

The buyer normally provides the reference or tells the seller what value to use. The seller should not invent a reference.

#### Is it mandatory?

Under the general Peppol rule **PEPPOL-EN16931-R003**, an invoice must contain either:

- A BuyerReference, or
- A purchase order reference.

However, under **DE-R-015**, if both the seller and buyer are located in Germany, a non-empty BuyerReference is required under the applicable German Peppol rules, even if a purchase order reference exists.

#### What if the buyer has not provided a purchase order?

A purchase order (PO) is created and issued by the buyer. A sales order is created by the seller.

If a seller's sales order reference is provided but there is no buyer purchase order reference, Peppol's specified convention allows `NA` in the mandatory purchase-order-reference field, with the seller's sales order number placed separately in `SalesOrderID`.

**Important:** `NA` is not a general placeholder for a missing BuyerReference. Do not use it to invent missing buyer information.

### EndpointID

#### What is it?

EndpointID is the electronic address associated with a party for electronic invoice exchange through Peppol.

When sending an invoice:

- The seller's EndpointID belongs in the seller section.
- The buyer's EndpointID belongs in the buyer section and identifies the intended recipient's electronic address.

#### How do you obtain it?

The buyer's EndpointID should be obtained from the buyer, their e-invoicing provider, or a trusted Peppol participant lookup.

The seller's own EndpointID is established when the seller registers their business with a Peppol service provider.

#### Is it mandatory?

Under Peppol BIS Billing 3.0:

- **PEPPOL-EN16931-R010** requires the buyer's electronic address.
- **PEPPOL-EN16931-R020** requires the seller's electronic address.
- **PEPPOL-EN16931-R008** prohibits empty elements.

A missing or empty EndpointID must therefore be treated as an error that needs to be resolved.

### EndpointID vs. PartyIdentification

These fields can sometimes contain the same value, but they have different purposes.

| Field | Purpose |
| --- | --- |
| `EndpointID` | The party's electronic address for invoice exchange |
| `PartyIdentification` | An identifier used to identify the business, such as a company or business-specific ID |
| `BuyerReference` | A reference used to route the invoice internally for the buyer |

**Important rule:** Having a company identifier does not prove that the company is registered on Peppol using that identifier.

Even if the scheme identifiers (`schemeID`) match, we must not assume that the `PartyIdentification` value is the correct EndpointID.
