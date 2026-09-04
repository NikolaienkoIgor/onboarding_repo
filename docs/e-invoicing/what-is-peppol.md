# What Is Peppol?

**Peppol** is a **secure international network for exchanging electronic business documents**, especially e-invoices.

Think of it like **a standardized postal system for businesses**:

> Instead of sending an invoice as a PDF by email, a company can send structured invoice data directly from its accounting/ERP system to the customer's system through Peppol.

## How Peppol Works

Peppol uses a **four-corner model**. A sender and receiver do not need to use the same software or service provider. Each organisation connects to one Peppol-certified Service Provider, which handles the exchange through the Peppol Network.

![Four-corner model](../assets/corner-models/4-corner.png)

## Peppol Is More Than a Network

Peppol consists of several layers that work together:

| Layer | What it does |
| --- | --- |
| **Peppol BIS** | Defines how business documents such as invoices are structured and exchanged |
| **Access Points** | Connect organisations to the Peppol Network |
| **Participant Directory (SMP / SML)** | Helps Peppol find a company and check which documents it can receive |
| **Security** | Provides secure and trusted document exchange |
| **Validation** | Checks whether documents comply with the applicable Peppol rules |

The Peppol framework combines document specifications, messaging, addressing, capability lookup, packaging and security requirements.

---

## Peppol BIS Billing 3.0

For e-invoicing, one of the most important specifications is **Peppol BIS Billing 3.0**.

It is based on the European semantic standard **EN 16931** and uses **UBL** as the primary XML syntax. Peppol BIS adds the specific implementation rules, restrictions, code lists and validation rules required for invoices exchanged through Peppol.

### The relationship is:

**EN 16931**

*Defines the meaning of the invoice data*

↓

**UBL**

*Defines the XML structure*

↓

**Peppol BIS Billing 3.0**

*Defines how the structure must be used and validated within Peppol*

For example, Peppol BIS specifies mandatory invoice information such as invoice number, issue date, currency, seller and buyer information, as well as business rules for calculations and coded values.
