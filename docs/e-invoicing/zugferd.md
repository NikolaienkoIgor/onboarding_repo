# Inside an E-Invoice: ZUGFeRD

**ZUGFeRD** is Germany's hybrid e-invoice format. It is a special PDF (**PDF/A-3**) that can contain other files inside it.

```
ZUGFeRD-Invoice.pdf   (PDF/A-3 container)
├── visible PDF (PDF/A-1)   — what a person sees
└── hidden XML file         — what a program reads
```

The visible PDF looks like a normal invoice. The hidden XML contains the same data — buyer, seller, line items, totals — in a structured form a machine can process without OCR.

*Simplified example of the hidden XML for the invoice in the previous chapter:*

```xml
<rsm:CrossIndustryInvoice ...>
    <rsm:ExchangedDocument>
        <ram:ID>Invoice Number 1234</ram:ID>
        <ram:IssueDateTime>
            <udt:DateTimeString format="102">20-03-2025</udt:DateTimeString>
        </ram:IssueDateTime>
    </rsm:ExchangedDocument>
    <ram:SupplyChainTradeTransaction>
        <ram:ApplicableHeaderTradeAgreement>
            <ram:SellerTradeParty>
                <ram:Name>Kumar's Electronic Shop</ram:Name>
            </ram:SellerTradeParty>
            <ram:BuyerTradeParty>
                <ram:Name>Anna Smith</ram:Name>
            </ram:BuyerTradeParty>
        </ram:ApplicableHeaderTradeAgreement>
        <ram:IncludedSupplyChainTradeLineItem>
            <ram:SpecifiedTradeProduct>
                <ram:Name>Laptop</ram:Name>
            </ram:SpecifiedTradeProduct>
        </ram:IncludedSupplyChainTradeLineItem>
    </ram:SupplyChainTradeTransaction>
</rsm:CrossIndustryInvoice>
```

The tag names used here (`SellerTradeParty`, `BuyerTradeParty`, etc.) belong to a vocabulary called **CII** (Cross Industry Invoice), one of two XML formats used for e-invoices.

!!! tip "Why hybrid works"
    The hybrid approach lets a person keep reading invoices normally while systems process the data underneath — this is the main advantage for small and medium businesses.
