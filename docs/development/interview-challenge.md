# Fintom8 AI Engineering Challenge: LangGraph Pre-ERP Invoice Validator

## 1. Overview & Context

At **Fintom8 AI**, we build agentic Pre-ERP document intelligence systems. Our microservices validate, correct, and enrich messy business document data before it touches core systems like SAP or DATEV.

In this coding challenge, you will design and implement a LangGraph-based validation agent in Python inside VS Code. Your agent will ingest a synthetic real-estate invoice PDF ([*AlpenHäuser Immobilien GmbH invoice*](../assets/invoices/Alpen_berens.pdf)) and run multi-step semantic and mathematical validations.

### The Task
Using **LangGraph** (and preferably **Google Gemini** via `LangChain` / `Google GenAI`), build an autonomous state machine that processes the invoice and executes the validation pipeline.

## 2. Detailed Validation Requirements

Your agent's graph must explicitly handle and evaluate the following **3 core criteria** against the provided AlpenHäuser Invoice PDF:

### 1) Verification of Sender & Recipient Data
* **Sender Checks:** Confirm company name, address, contact info, and tax/banking metadata (German VAT ID, IBAN, BIC).
* **Recipient Checks:** Confirm customer name, full address, and VAT ID.
* **Validation Outcome:** Flag any missing mandatory billing attributes.

### 2) Verification of Core Mathematical Calculations
* **Line-Item Math:** Validate `Total Net` for each line item.
* **Aggregation Check:** Verify whether the sum of line items correctly matches the stated Net Total.

### 3) Line-Item Tax Calculation & Anomaly Detection
* **Tax Breakdown & Verification:** Calculate and validate the tax applied to each line item based on standard German VAT rates:
    * Taxable items (standard / reduced rates)
    * Exempt / Security deposit items
    * Tax Sum Check: Confirm the aggregated VAT amount matches the stated total VAT.
    * Gross Check: Confirm that `Net Total` + `VAT` = `Gross Total`.

## 3. Technical Requirements & Setup

* **IDE / Environment:** Implement your project in Visual Studio Code (VS Code).
* **Core Libraries:**
     * `langgraph`
     * `langchain-google-genai` (or `google-generativeai` / alternate LLM provider)
     * `pydantic` *(optional, for structured state and schema definition)*
     * `pypdf` / `pdfplumber` *(optional, to parse the PDF document)*
* **Agent Output:** The agent should output a structured (JSON, Pydantic object or Terminal report) indicating:
     * Extracted document payload.
     * Step-by-step validation status (`PASSED` / `FAILED`).
     * Diagnostic logs explaining discrepancies or confirming full compliance.

## 4. Submission Deliverables

As outlined in our hiring guidelines, please provide:

1. **GitHub Repository Link:**
   * Clean, well-documented code containing your LangGraph definition, state types, nodes, and instructions on how to run it.

2. **Screen Recording (2–5 minutes):**
   * Demonstrate the code running end-to-end inside VS Code.
   * Walk through your graph structure (nodes, edges, state definition).
   * Show the agent executing validation on the provided AlpenHäuser PDF.
   * Briefly discuss future extensions you would add (e.g., auto-repair, EN16931 Schematron validation, ERP export).
   * Upload the video to Google Drive (with public view permissions) and include the link.

> *Have fun building! We don't expect perfection—we are looking for clean problem-solving, structured graph logic, and strong foundational Python skills.*
