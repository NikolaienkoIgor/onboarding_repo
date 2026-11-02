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
| 12 | **Missing Seller Endpoint ID** *(BT-34)* | `PEPPOL-EN16931-R020` | The seller's electronic address (`cbc:EndpointID`) is missing under `cac:AccountingSupplierParty`. Peppol requires this address to identify the seller's electronic endpoint. |
| 13 | **Missing Buyer Endpoint ID** *(BT-49)* | `PEPPOL-EN16931-R010` | The buyer's electronic address (`cbc:EndpointID`) is missing under `cac:AccountingCustomerParty`. Peppol requires this address to identify the buyer's electronic endpoint. |
