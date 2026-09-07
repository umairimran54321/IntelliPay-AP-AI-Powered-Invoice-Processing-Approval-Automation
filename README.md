# IntelliPay-AP-AI-Powered-Invoice-Processing-Approval-Automation
IntelliPay AP is an AI-powered Accounts Payable automation system designed to streamline the complete invoice processing and approval lifecycle — from invoice intake and data extraction to Purchase Order (PO) validation, Vendor ID matching, VAT calculation, exception detection, approval routing, and final processing.

The project combines **AI agents, workflow automation, structured data validation, and business rules** to reduce manual Accounts Payable work and improve invoice processing accuracy.

Instead of manually reviewing invoices against purchase orders, IntelliPay AP automates the repetitive verification process and identifies invoices that require human intervention.

---

## 🚀 Project Overview

Accounts Payable teams often spend significant time manually processing invoices, checking purchase orders, verifying vendor information, calculating VAT, and determining whether an invoice should be approved or rejected.

IntelliPay AP addresses this problem by creating an automated invoice-processing pipeline.

The system receives an invoice, extracts the relevant information, validates it against the organization's PO database, checks the Vendor ID and purchased items, verifies quantities and unit prices, calculates VAT, and determines the appropriate next action.

### Core workflow

**Invoice → AI Extraction → PO Lookup → Vendor Validation → Line-Item Matching → VAT Validation → Business Rules → Approval / Exception Handling**

The objective is to turn a traditionally manual AP process into a structured, auditable, and intelligent automation workflow.

---

# ✨ Key Features

## 1. AI-Powered Invoice Data Extraction

The system can process invoice information and identify important fields such as:

* Invoice Number
* Invoice Date
* Due Date
* PO Number
* Vendor ID
* Vendor Name
* Item Description
* Item Code
* Quantity
* Unit Price
* Subtotal
* VAT
* Total Amount
* Payment Terms
* Currency

The extracted information is converted into structured data that can be used by downstream automation steps.

---

## 2. Purchase Order Matching

One of the most important features of IntelliPay AP is automated **PO-to-Invoice matching**.

The system verifies that the invoice references a valid Purchase Order and compares invoice information against the corresponding PO.

For example:

| PO Field       | Invoice Field    |
| -------------- | ---------------- |
| PO Number      | PO Number        |
| Vendor ID      | Vendor ID        |
| Item           | Item Description |
| Quantity       | Quantity         |
| Unit Price     | Unit Price       |
| Total          | Invoice Subtotal |
| VAT            | Calculated VAT   |
| Total With VAT | Invoice Total    |

This prevents invoices from being processed against incorrect or nonexistent purchase orders.

---

## 3. Vendor ID Validation

Every invoice is associated with a Vendor ID.

IntelliPay AP verifies that:

* The PO exists
* The Vendor ID exists
* The Vendor ID belongs to the referenced PO
* The invoice Vendor ID matches the PO Vendor ID

For example:

```text
PO Number: PO-88331
Vendor ID: V009
Item: Water Pumps
```

If an invoice contains a different Vendor ID for the same PO, the system can flag the invoice as an exception instead of automatically approving it.

---

## 4. Line-Item Validation

The system validates individual invoice line items against the PO.

Validation can include:

* Item name
* Item code
* Quantity
* Unit price
* Line total
* Number of line items

For example:

```text
PO Quantity: 20
Invoice Quantity: 20

PO Unit Price: 650
Invoice Unit Price: 650

PO Total: 13,000
Invoice Total: 13,000
```

When the values match, the invoice passes the corresponding validation rules.

---

## 5. Automated VAT Calculation

IntelliPay AP automatically calculates VAT using the configured VAT rate.

For example:

```text
Subtotal = SAR 13,000

VAT Rate = 15%

VAT = 13,000 × 0.15
VAT = SAR 1,950

Total With VAT = 13,000 + 1,950
Total With VAT = SAR 14,950
```

The calculated VAT can then be compared against the VAT amount provided on the invoice.

---

## 6. Invoice Total Validation

The system verifies that invoice totals are mathematically correct.

The validation follows:

```text
Line Total = Quantity × Unit Price

Subtotal = Sum of Line Totals

VAT = Subtotal × VAT Rate

Grand Total = Subtotal + VAT
```

This allows the workflow to detect calculation discrepancies before an invoice moves forward.

---

# 📋 Example PO Validation

The project includes a PO dataset containing records such as:

| PO Number | Vendor ID | Item               | Quantity | Unit Price |  Total | Total With VAT |
| --------- | --------- | ------------------ | -------: | ---------: | -----: | -------------: |
| PO-88321  | V001      | HVAC Filters       |      100 |         25 |  2,500 |          2,875 |
| PO-88322  | V002      | Equipment          |       20 |        150 |  3,000 |          3,450 |
| PO-88326  | V002      | Power Tools        |       15 |        320 |  4,800 |          5,520 |
| PO-88331  | V009      | Water Pumps        |       20 |        650 | 13,000 |         14,950 |
| PO-88333  | V010      | Air Compressors    |       15 |        850 | 12,750 |      14,662.50 |
| PO-88336  | V007      | Computer Monitors  |       50 |        290 | 14,500 |         16,675 |
| PO-88339  | V010      | Backup Batteries   |       75 |        175 | 13,125 |      15,093.75 |
| PO-88340  | V001      | AC Condenser Units |       15 |        950 | 14,250 |      16,387.50 |

The workflow uses these PO records as the reference source when validating incoming invoices.

---

# 🧾 Example Invoice Validation

Consider an invoice containing:

```text
Invoice Number: INV-2026-10427
PO Number: PO-88331
Vendor ID: V009

Item: Water Pumps
Quantity: 20
Unit Price: SAR 650

Subtotal: SAR 13,000
VAT 15%: SAR 1,950
Total: SAR 14,950
```

The workflow retrieves:

```text
PO-88331
Vendor ID: V009
Item: Water Pumps
Quantity: 20
Unit Price: SAR 650
PO Total: SAR 13,000
Total With VAT: SAR 14,950
```

The system then compares the invoice and PO.

### Result

```text
PO Match: PASS
Vendor Match: PASS
Item Match: PASS
Quantity Match: PASS
Unit Price Match: PASS
Subtotal Match: PASS
VAT Validation: PASS
Grand Total Validation: PASS
```

The invoice can therefore proceed to the next stage of the approval workflow.

---

# 🤖 AI Agent Layer

IntelliPay AP uses AI to handle invoice understanding and intelligent decision-making.

The AI layer can interpret semi-structured invoice information and convert it into predictable structured fields.

Example:

```json
{
  "invoice_number": "INV-2026-10427",
  "po_number": "PO-88331",
  "vendor_id": "V009",
  "item": "Water Pumps",
  "quantity": 20,
  "unit_price": 650,
  "subtotal": 13000,
  "vat_rate": 15,
  "vat_amount": 1950,
  "total": 14950
}
```

This structured output can then be passed to deterministic validation and automation steps.

---

# 🔍 Exception Detection

Not every invoice should be automatically approved.

IntelliPay AP is designed to detect discrepancies such as:

### Invalid PO

```text
Invoice PO: PO-99999
PO Database: PO not found
```

### Vendor mismatch

```text
Invoice Vendor ID: V003
PO Vendor ID: V009
```

### Quantity mismatch

```text
PO Quantity: 20
Invoice Quantity: 25
```

### Unit price mismatch

```text
PO Unit Price: SAR 650
Invoice Unit Price: SAR 700
```

### VAT mismatch

```text
Expected VAT: SAR 1,950
Invoice VAT: SAR 2,100
```

### Total mismatch

```text
Expected Total: SAR 14,950
Invoice Total: SAR 15,100
```

These exceptions can be routed for manual review rather than allowing the invoice to continue automatically.

---

# 🔄 Automated Approval Logic

The workflow can classify invoices according to validation results.

### Example

```text
IF
PO exists
AND Vendor ID matches
AND Item matches
AND Quantity matches
AND Unit Price matches
AND VAT is correct
AND Total is correct

THEN
→ Invoice Valid
→ Send for Approval
```

Otherwise:

```text
→ Invoice Exception
→ Identify discrepancy
→ Route for Manual Review
```

This creates a clear separation between **straight-through processing** and **exception handling**.

---

# 🏗️ Architecture

The overall architecture can be represented as:

```text
                ┌─────────────────────┐
                │   Invoice Input     │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │   AI Extraction     │
                │   & Classification  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Structured Invoice  │
                │       Data          │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │     PO Lookup       │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Vendor ID Matching  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ Line Item Matching  │
                └──────────┬──────────┘
                           │
                           ▼
                ┌─────────────────────┐
                │ VAT & Total Check   │
                └──────────┬──────────┘
                           │
                  ┌────────┴────────┐
                  ▼                 ▼
          ┌───────────────┐  ┌───────────────┐
          │    Valid      │  │   Exception   │
          │    Invoice    │  │    Detected   │
          └───────┬───────┘  └───────┬───────┘
                  │                  │
                  ▼                  ▼
          ┌───────────────┐  ┌───────────────┐
          │   Approval    │  │ Manual Review │
          └───────────────┘  └───────────────┘
```

---

# 🧠 AI + Deterministic Automation

A major design principle of IntelliPay AP is combining **AI reasoning with deterministic business rules**.

AI is useful for:

* Understanding invoice documents
* Extracting information
* Identifying invoice fields
* Classifying invoice information
* Handling semi-structured data

Deterministic logic is better suited for:

* PO existence checks
* Vendor ID matching
* Quantity comparisons
* Price comparisons
* VAT calculations
* Total calculations
* Approval thresholds

This approach helps prevent an AI model from inventing financial values or making uncontrolled calculations.

---

# ⚡ Benefits

IntelliPay AP can help organizations:

* Reduce manual invoice processing
* Reduce data-entry errors
* Automate PO matching
* Validate vendor information
* Detect invoice discrepancies
* Automate VAT calculations
* Improve invoice processing speed
* Standardize AP validation
* Reduce repetitive administrative work
* Create a structured approval process
* Improve auditability
* Route exceptions automatically

---

# 🔐 Validation & Reliability

Financial automation requires predictable validation.

For this reason, IntelliPay AP separates:

**AI interpretation**

from

**business-rule validation**

The AI can extract and structure information, while deterministic workflow logic performs the final numerical and reference checks.

This architecture reduces the risk of approving an invoice simply because an AI model interpreted the document incorrectly.

---

# 🛠️ Technology Stack

Depending on the implementation, the project can integrate technologies such as:

* **n8n** — Workflow automation
* **AI / LLMs** — Invoice understanding and extraction
* **JavaScript** — Data transformation and validation
* **JSON** — Structured invoice data
* **Google Sheets / Database** — PO and vendor reference data
* **Webhooks / APIs** — Invoice and system integrations
* **Email automation** — Notifications and approval communication

---

# 📊 Example Processing Flow

### Step 1 — Invoice Received

The system receives an invoice through the configured input channel.

### Step 2 — Invoice Information Extracted

AI identifies the invoice number, PO number, Vendor ID, item, quantity, pricing, VAT, and total.

### Step 3 — PO Retrieved

The workflow searches the PO database using the invoice PO number.

### Step 4 — Vendor Validated

The invoice Vendor ID is compared with the Vendor ID assigned to the PO.

### Step 5 — Items Compared

Quantity, item description, and unit price are checked against the PO.

### Step 6 — Financial Values Validated

The workflow calculates subtotal, VAT, and final invoice total.

### Step 7 — Decision Made

The invoice is classified as either:

```text
VALID
```

or

```text
EXCEPTION
```

### Step 8 — Approval / Exception Routing

Valid invoices proceed through the approval process while discrepancies are routed for manual review.

---

# 🎯 Project Goal

The primary goal of IntelliPay AP is to demonstrate how **AI agents and workflow automation can be combined to build an intelligent Accounts Payable system**.

Rather than simply extracting invoice information, the system focuses on the complete decision pipeline:

**Extract → Match → Validate → Calculate → Decide → Approve**

This makes the project applicable to real-world procurement and finance automation use cases.

---

# 🚀 Future Enhancements

Potential future improvements include:

* OCR-based PDF invoice processing
* Multi-invoice batch processing
* Three-way matching between PO, invoice, and goods receipt
* Automated approval hierarchy
* Vendor master database
* Duplicate invoice detection
* Fraud/anomaly detection
* ERP integrations
* SAP integration
* Microsoft Dynamics integration
* QuickBooks integration
* Automated payment initiation
* Approval dashboards
* Invoice status tracking
* Audit logs
* Confidence scoring
* Human-in-the-loop review
* Email-based approval workflows
* Slack / Microsoft Teams approval notifications

---

# 📌 Use Case

IntelliPay AP is particularly suitable as a foundation for organizations that process large numbers of supplier invoices and want to automate repetitive Accounts Payable operations.

It demonstrates how AI can be integrated with traditional workflow automation to create a more efficient and reliable invoice-processing pipeline.

---

## 👨‍💻 Project

**IntelliPay AP — AI-Powered Invoice Processing & Approval Automation**

**Category:** AI Automation / FinTech / Accounts Payable / Intelligent Document Processing

**Core Concept:**
Automating invoice extraction, PO matching, vendor validation, VAT verification, discrepancy detection, and approval routing using AI and workflow automation.

---

## ⭐ Key Takeaway

IntelliPay AP transforms invoice processing from a manual verification task into an intelligent automated workflow:

> **Invoice → AI → PO Matching → Vendor Validation → Financial Validation → Decision → Approval**

The project demonstrates the practical use of **AI agents, workflow automation, structured data processing, and deterministic financial validation** to solve a real-world business problem.
## Workflow Screenshots
## Approve

![IntelliPay-AP-Approve.jpeg]{IntelliPay-AP-Approve.jpeg}

## Reject
![IntelliPay-AP-Reject.jpeg]{IntelliPay-AP-Reject.jpeg}

## Human Approve
![IntelliPay-AP-Human-Approve.jpeg]{IntelliPay-AP-Human-Approve.jpeg}

## Error
![IntelliPay-AP-Error.jpeg]{IntelliPay-AP-Error.jpeg}

## Duplicate
![IntelliPay-AP-Duplicate.jpeg]{IntelliPay-AP-Duplicate.jpeg}

## AI Email Approve 
![IntelliPay-AP-AI-Email-Approve.jpeg]{IntelliPay-AP-AI-Email-Approve.jpeg}

## AI Email Reject
![IntelliPay-AP-AI-Email-Reject.jpeg]{IntelliPay-AP-AI-Email-Reject.jpeg}

## Human Approval Email
![IntelliPay-AP-Human-Email.jpeg]{IntelliPay-AP-Human-Email.jpeg}

## Huaman Approval Form
![IntelliPay-AP-Human-Email(2).jpeg]{IntelliPay-AP-Human-Email(2).jpeg}
