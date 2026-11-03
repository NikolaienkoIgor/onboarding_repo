# **E-Transport in Europe: The difference between CMR and eFTI**

## **What is "E-Transport"?**

There is no single EU-wide definition. The term covers two things:

1. **National tax-control systems** for road freight. Romania's is literally called RO e-Transport.  
2. **The EU's eFTI framework**, which digitalises freight documents such as the e-CMR.

Both have the same basic idea. Goods movements become structured, machine-readable data that authorities can check.

## **Why it exists**

The national systems aim to fight VAT fraud. Romania's tax authority, ANAF, wants to reduce the VAT gap through real-time monitoring of goods movements on national roads. The EU framework has a different goal. It wants to replace paper with standardised electronic data, cutting administrative cost for businesses and authorities.

## **The EU layer: eFTI**

Regulation (EU) 2020/1056 creates a common legal framework for electronic freight transport information. Its main rules are:

* **Authorities:** from 9 July 2027, Member State authorities must accept information shared electronically via certified eFTI platforms.  
* **Businesses:** adoption remains voluntary. Paper stays permitted.  
* **Platforms:** data flows through certified eFTI platforms and national "gates". The Commission's implementing and delegated acts on the common data set and authority procedures were adopted in 2024\.  
* **Expected benefit:** the Commission estimates savings of up to €1 billion per year for the transport and logistics sector.

## **eFTI and CMR: how they relate**

The CMR Convention of 1956 is an international treaty for road freight. It governs the contract of international carriage by road and defines what the CMR consignment note contains. eFTI is not a replacement for it.

* **Different roles.** CMR sets the content of the consignment note: sender, consignee, carrier, goods and place of delivery. eFTI sets how information of this kind is exchanged electronically with authorities.
* **Shared data.** Data from CMR documents can be used to supply transport information through eFTI platforms. Authorities then read the relevant data electronically instead of checking paper.
* **Example.** On a shipment from Germany to France, the relevant CMR data can be submitted through an eFTI platform for inspection by the authorities.

## **The national pioneers**

|  | Romania (RO e-Transport) | Poland (SENT) | Hungary (EKAER) |
| ----- | ----- | ----- | ----- |
| Approach | Declare each shipment in advance and receive a UIT code | Register shipments and transmit GPS location | Request an EKAER number before transport |
| Scope | High-fiscal-risk goods, plus international road transport | "Sensitive" goods such as fuel, alcohol, tobacco and waste | Road transport above weight and value limits |
| Portal | ANAF's SPV | PUESC | NAV's EKAER portal |

## **Why it matters: logistics and finance converge**

Three developments point in the same direction:

1. The transport document proves physical movement of the goods.  
2. The invoice is a structured data record of the same transaction.  
3. Tax authorities increasingly compare the two. Romania already links e-Transport with its e-invoicing system. Other countries run the two systems side by side.

For companies this means logistics systems (TMS) and finance systems (ERP) need clean, consistent data. Order, delivery note and invoice should tell the same story. This follows from the direction of regulation, not from a single legal requirement.

## **What companies should do now**

* **Map where they operate.** Check which national systems apply to their routes and goods.  
* **Prepare for 2027\.** Review eFTI platform options before authorities must accept the data in July 2027\.  
* **Align logistics and invoicing data** so the two can be compared automatically.  
* **Plan for 2030\.** The ViDA e-invoicing and reporting deadline comes with it.

**Sources to E-Transport**:   
[https\://openlogisticsfoundation.org/efti/](https://openlogisticsfoundation.org/efti/)  
[https\://www\.logifie.com/de/blog/efti-regulation-road-freight-europe-guide](https://www.logifie.com/de/blog/efti-regulation-road-freight-europe-guide)  
[https\://transport.ec.europa.eu/news-events/news/towards-paperless-freight-transport-eu-takes-step-forward-efti-regulation-implementation-2025-01-09\_en](https://transport.ec.europa.eu/news-events/news/towards-paperless-freight-transport-eu-takes-step-forward-efti-regulation-implementation-2025-01-09_en)  
[https://www.unidroit.org/instruments/transport/cmr-1956/](https://www.unidroit.org/instruments/transport/cmr-1956/)

### **1\. When does it become mandatory in Germany?**

There are two different things, with different dates.

**E-invoicing (domestic B2B)**

* **Receiving:** companies must be able to receive e-invoices since 1 January 2025\.   
* **Issuing:** mandatory from 1 January 2027 for larger companies and from 1 January 2028 for smaller ones. The threshold is 800,000 € prior-year turnover.  
* **Cross-border:** the EU-wide ViDA obligation starts on 1 July 2030\. A German transaction reporting system is expected from around 2028, but that date is not final.

**E-Transport (freight documents)**

* Germany has no national tax-control system like RO e-Transport, SENT or EKAER.  
* The EU framework, eFTI, requires authorities to accept eFTI data from July 2027, but it stays voluntary for businesses. So there is no date on which German hauliers must go digital.


### **2\. Why E-Transport suits startups better than large software companies**

This section is my analysis, grounded in the sources. It is not established fact, and there are real counterarguments below.

**Arguments for startups**

1. **The rules are fragmented, which favours specialists.** There are separate national systems with different portals, thresholds and penalties. Romania, for example, has weight and value thresholds and UIT code validity periods, and Poland’s SENT list keeps growing, for example with clothing and footwear from March 2026\. Keeping up with this for several countries is detailed work. It is not an obvious priority for a large vendor.  
2. **Early movers can shape the market.** The Open Logistics Foundation notes that companies that get involved early can help shape the solution. The eFTI platform certification rules were also still being finalised late, so the field is not yet locked up.  
3. **Entry costs are lower than they used to be.** Open-source building blocks exist. Germany’s pilots are developing an open-source eFTI platform reference implementation for eCMR. A startup does not need to build everything from scratch.  
4. **The customers are mostly small.** Industry voices call for a balance between small and large companies, because the haulage sector has many small operators. Enterprise TMS vendors usually optimise for large accounts. Small hauliers need cheap, simple tools.  
5. **It connects to e-invoicing.** One provider already pitches eCMR combined with the creation of electronic invoices. A product that links transport data and invoice data across countries fits a startup’s focus well.  
6. **Timing is good.** Stakeholders expect large-scale eCMR implementation to start in 2026, with full adoption around 2029\.

**Arguments against, and risks**

* **Demand is uncertain.** eFTI is voluntary for businesses, so adoption depends on perceived benefit rather than a legal deadline.  
* **Certification is a burden.** Platforms must be certified, and buyers also ask for things like ISO 27001\. This costs money and time that a small team may not have.  
* **Incumbents are moving.** Specialised eCMR providers and IT service companies already announce eFTI platforms, for example EDITEL aims to be among the first. Large software companies can add the function to products they already sell.  
* **Interoperability cuts both ways.** Common standards make switching easier for customers, which reduces lock-in for everyone.  
* **National rules change often.** Maintaining country-specific integrations is a continuous cost.

**Sources to eFTI**: 
[Official Journal of the European Union](https://eur-lex.europa.eu/legal-content/EN/TXT/PDF/?uri=OJ:L_202402024)