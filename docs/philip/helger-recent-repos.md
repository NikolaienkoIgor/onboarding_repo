# Helger’s 10 most recently pushed repos

Cheat sheet for the repositories Philip Helger ([`phax`](https://github.com/phax)) pushed to last, as of **10 Sep 2026**. Ordered by last git push. Most of these sit in the Peppol / e-invoicing stack; one (`ph-totp`) does not.

---

## 1. [phoss-smp](https://github.com/phax/phoss-smp)

**Last push:** 10 Sep 2026, 12:18 UTC

**In one line:** A production Peppol *address book server*.

Peppol is a four-corner network. Before you send an invoice you must look up “where does this participant receive documents, and with which certificate?” That lookup hits an **SMP** (Service Metadata Publisher). `phoss-smp` *is* that server. Helger states it runs a large share of live Peppol SMPs. It also speaks OASIS BDXR SMP and EC eDelivery.

Think: DNS for e-invoices — “who is `9915:acme` and what URL do I POST to?”

---

## 2. [peppol-commons](https://github.com/phax/peppol-commons)

**Last push:** 10 Sep 2026, 10:10 UTC

**In one line:** Shared Peppol building blocks used by almost everything else.

Not a product you run. A Java library of the boring-but-critical bits:

- participant / document-type / process identifiers
- official code lists
- **SMP client** (query an SMP)
- **SML client** (register participants in the Service Metadata Locator)
- SBDH rules for Peppol
- MLR / MLS helpers (message-level response / status)

If `phoss-smp` is the address book server, this is the *client SDK* plus identifier types.

---

## 3. [phase4](https://github.com/phax/phase4)

**Last push:** 10 Sep 2026, 10:09 UTC

**In one line:** The *postal service* for Peppol — send and receive AS4 messages.

AS4 is the encrypted SOAP/HTTP protocol Peppol Access Points use. `phase4` is a library (not a full product UI) that:

- wraps your UBL/CII invoice in the right envelope
- signs and encrypts it
- talks AS4 to the other party’s Access Point
- has Peppol and CEF eDelivery profiles built in

`phoss-ap` (not in this top-10 list) is the full Access Point product built *on top of* phase4.

---

## 4. [ddd](https://github.com/phax/ddd) — Document Details Determinator

**Last push:** 10 Sep 2026, 09:06 UTC

**In one line:** Looks at an XML invoice and answers “what *kind* of document is this, and which validation rules apply?”

You hand it the XML root. It reads namespace + element name + `CustomizationID` and returns:

- syntax (UBL Invoice, CII, XRechnung, fatturaPA, …)
- profile name
- **VESID** — the ID of the Schematron pack to run in `phive`

It can also peel off SBDH / XHE envelopes so you validate the inner document, not the wrapper.

Think: a file-magic / content-type detector, but for e-invoices.

---

## 5. [phoss-directory](https://github.com/phax/phoss-directory)

**Last push:** 9 Sep 2026, 21:02 UTC

**In one line:** Software behind the public Peppol Directory (the “yellow pages”).

This is what powers [directory.peppol.eu](https://directory.peppol.eu): search for a company and see that they are on Peppol. Different from an SMP:

| | SMP | Directory |
|---|---|---|
| Question | *How* do I send to this ID? (URL, cert, doc types) | *Does this company exist* on Peppol? (name, country, participant ID) |
| Audience | Access Points | Humans / business lookup |

---

## 6. [phive-rules-shared](https://github.com/phax/phive-rules-shared)

**Last push:** 9 Sep 2026, 20:56 UTC

**In one line:** Glue code shared by all the country/rule packs in `phive-rules`.

`phive` = validation *engine*. `phive-rules` = the actual Peppol / EN 16931 / XRechnung / … Schematron files, split into many Maven modules. This repo holds the common SPI and helpers those modules all depend on, so Helger can version “the rules” and “the shared code” separately.

You would not run this alone. You would depend on `phive-rules-peppol` etc., which pull this in.

---

## 7. [ph-totp](https://github.com/phax/ph-totp)

**Last push:** 9 Sep 2026, 20:49 UTC · **fork** (not Peppol)

**In one line:** Google Authenticator-style 6-digit login codes (TOTP) for Java.

RFC 6238: generate a secret, show a QR code, verify the rotating code, optional recovery codes, optional NTP clock. Forked from `samdjstevens/java-totp`, relicensed Apache 2.0. Unrelated to invoices — login MFA for any app (for example an SMP or AP admin UI).

---

## 8. [ph-xhe](https://github.com/phax/ph-xhe)

**Last push:** 9 Sep 2026, 20:46 UTC

**In one line:** Read/write the **XHE** envelope (the wrapper *around* the invoice).

Peppol usually wraps the business document in **SBDH**. Some other networks (notably DBNAlliance) use OASIS **XHE** (Exchange Header Envelope) instead. Same idea: “this payload is an invoice, from A, to B.” This library is the JAXB model so Java can parse and emit that XML header.

---

## 9. [ph-sbdh](https://github.com/phax/ph-sbdh)

**Last push:** 9 Sep 2026, 20:46 UTC

**In one line:** Read/write the **SBDH** envelope Peppol actually uses.

UN/CEFACT Standard Business Document Header. When an Access Point sends a UBL invoice over AS4, the invoice sits *inside* an SBDH. This library maps that header XML to Java objects. Peppol-specific extra rules on top of SBDH live in `peppol-commons`, not here.

**SBDH vs XHE:** two competing “envelope” formats. Peppol → SBDH. Some other eDelivery networks → XHE. Helger maintains both.

---

## 10. [ph-regrep](https://github.com/phax/ph-regrep)

**Last push:** 9 Sep 2026, 20:46 UTC

**In one line:** Read/write OASIS **ebXML Registry (RegRep 4)** XML.

RegRep is a generic “registry of objects and metadata” standard (catalogues, classifications), not an invoice format. Helger’s other eDelivery/SMP work touches registry-style documents; this is the JAXB wrapper so those XML files can be loaded and saved in Java.

---

## How they fit together (Peppol send path)

```
Invoice XML
    → ddd              “this is Peppol BIS Billing UBL, use VESID X”
    → phive-rules*     Schematron check (uses phive-rules-shared)
    → ph-sbdh          wrap in SBDH header
    → peppol-commons   look up receiver via SMP client
    → phase4           send AS4 to their Access Point
         ↑
    phoss-smp          (the SMP server the client queried)
    phoss-directory    (optional: human-searchable yellow pages)
```

`ph-xhe` is the same wrap step for networks that are not SBDH. `ph-totp` and `ph-regrep` sit off to the side (login MFA; registry XML).

Pushes on 9 Sep 20:46 UTC for `ph-xhe`, `ph-sbdh`, and `ph-regrep` landed in the same minute — likely a coordinated library release, not three separate features.
