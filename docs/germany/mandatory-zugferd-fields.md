# Mandatory ZUGFeRD Fields(EN 16931 / § 14 UStG)

 For an electronic invoice to be legally valid and technically accepted by automated validators, all relevant statutory fields and semantic Business Terms (BTs) must be populated directly within the structured XML file.

## 1\. Document-Level and Header Metadata

**The structured invoice must contain the information needed to identify the document, the parties, the transaction and the payment conditions.**

| Business Term | Semantic Field | Purpose |
| ----- | ----- | ----- |
| **BT-1** | Invoice number | Unique identifier assigned to the invoice |
| **BT-2** | Invoice issue date | Date on which the invoice is issued |
| **BT-3** | Invoice type code | Identifies the document type, such as commercial invoice or corrected invoice |
| **BT-5** | Invoice currency code | ISO 4217 currency code, such as EUR |
| **BT-10** | Buyer reference | Buyer-specific reference or routing information |
| **BT-23** | Business process type | Identifies the applicable business process |
| **BT-24** | Specification identifier | Identifies the invoice specification or profile |
| **BT-9** | Payment due date | Date on which payment becomes due |
| **BT-20** | Payment terms | Additional payment conditions |

Not every EN 16931 Business Term is mandatory for every invoice. **Whether a BT is mandatory, optional, or conditional depends on the applicable syntax, implementation specification, transaction type, and business rules.**

## 2\. Seller and Buyer Information

**The invoice must identify the supplier and customer sufficiently to satisfy both the VAT requirements and the applicable electronic invoice specification.**

| Business Term | Semantic Field | Purpose |
| ----- | ----- | ----- |
| **BT-27** | Seller name | Legal or business name of the supplier |
| **BT-44** | Buyer name | Legal or business name of the customer |
| **BT-35–40** | Seller address | Seller's postal address |
| **BT-50–55** | Buyer address | Buyer's postal address |
| **BT-31** | Seller VAT identifier | Supplier's VAT identification number where applicable |
| **BT-32** | Seller tax registration identifier | Domestic tax number or equivalent identifier where applicable |
| **BT-48** | Buyer VAT identifier | Customer VAT ID where required |
| **BT-34** | Seller electronic address | Electronic address used for invoice exchange |
| **BT-49** | Buyer electronic address | Customer electronic address |

The electronic address is particularly important in automated networks because the recipient must be identifiable as a machine-processing endpoint.

For B2G transactions, the buyer's required routing identifier can be specified separately from the general buyer-reference concept.

## 3\. Supply Information and Invoice Line Items

**Invoice lines provide the structured explanation of what was supplied, in what quantity, at what price, and under which VAT treatment.**

| Business Term | Semantic Field | Purpose |
| ----- | ----- | ----- |
| **BT-72** | Actual delivery date | Date of delivery or performance |
| **BT-73–74** | Delivery period | Start and end of a supply period |
| **BT-126** | Invoice line identifier | Unique line sequence |
| **BT-153** | Item name | Description of the good or service |
| **BT-129** | Invoiced quantity | Quantity supplied |
| **BT-130** | Unit code | Standard unit code |
| **BT-131** | Invoice line net amount | Net value of the individual line |
| **BT-146** | Item net price | Net unit price |
| **BT-147** | Item price discount | Discount applied at line level |
| **BT-148** | Item gross price | Gross price before applicable line discount |
| **BT-149** | Price base quantity | Quantity used as the pricing basis |
| **BT-150** | Price base quantity unit | Unit associated with the price basis |
| **BT-151** | VAT category | VAT classification |
| **BT-152** | VAT rate | Applicable VAT rate |

Examples of standardized units include **C62** for a piece/unit and **HUR** for an hour.

The system should calculate line amounts from controlled source data rather than relying on manually edited totals. Quantity, unit, price, discounts, tax category and line amount should remain mathematically consistent.

## 4\. VAT Breakdown and Monetary Totals

**The VAT structure must allow the recipient and accounting system to understand how the invoice amount was calculated.**

| Business Term | Semantic Field | Purpose |
| ----- | ----- | ----- |
| **BT-106** | Sum of invoice line net amounts | Total of all line-level net amounts |
| **BT-107** | Document-level allowance | Header-level discount |
| **BT-108** | Document-level charge | Header-level surcharge |
| **BT-109** | Invoice total without VAT | Net invoice amount |
| **BT-110** | Invoice total VAT amount | Total VAT amount |
| **BT-112** | Invoice total with VAT | Gross invoice amount |
| **BT-115** | Amount due for payment | Final amount payable |
| **BT-116** | VAT taxable amount | Taxable base for a VAT category |
| **BT-117** | VAT amount | Tax calculated for the VAT category |
| **BT-118** | VAT category code | VAT classification |
| **BT-119** | VAT rate | Applicable tax percentage |

The monetary model should remain internally consistent:

**Line amounts → document allowances/charges → taxable amounts → VAT amounts → invoice total → amount due**

A technically valid XML file can still represent an incorrect invoice if these values do not correspond to the underlying transaction.
