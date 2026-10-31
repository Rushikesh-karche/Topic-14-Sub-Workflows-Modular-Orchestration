# Topic 14: Sub-Workflows, Debugging & Production Readiness
**n8n Hands-On Assessment - Level 2**

[![n8n Version](https://img.shields.io/badge/n8n-v1.0%2B-EA4B71?logo=n8n)](https://n8n.io/)
[![Status](https://img.shields.io/badge/Status-Completed-success)](https://github.com/Rushikesh-karche/Topic-14-Sub-Workflows-Modular-Orchestration)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📌 Project Overview
This repository contains the complete implementation for **Topic 14: Sub-Workflows, Debugging & Production Readiness**. It demonstrates an enterprise modular microservice architecture in n8n.

The Master Orchestrator coordinates multi-step order processing by delegating specialized domain operations to independent child sub-workflows:
1. **Sub-Workflow 1 (Tax Calculation Engine):** Enforces strict input contract validation and applies state-specific tax rates.
2. **Sub-Workflow 2 (Payment Gateway Processor):** Authorizes financial charges and issues transaction receipts.

---

## 📂 Repository Contents
| File | Description |
| :--- | :--- |
| **[`Topic 14 - Sub-Workflows & Modular Orchestrator.json`](./Topic%2014%20-%20Sub-Workflows%20%26%20Modular%20Orchestrator.json)** | Complete Master Orchestrator workflow with pre-configured pinData |
| **[`Topic 14 - Sub-Workflow 1 Tax Engine.json`](./Topic%2014%20-%20Sub-Workflow%201%20Tax%20Engine.json)** | Standalone Tax Calculation Engine child sub-workflow |
| **[`Topic 14 - Sub-Workflow 2 Payment Gateway.json`](./Topic%2014%20-%20Sub-Workflow%202%20Payment%20Gateway.json)** | Standalone Payment Gateway Processor child sub-workflow |
| **[`n8n_topic_14_requirement_document.md`](./n8n_topic_14_requirement_document.md)** | Full technical requirement specification & Docker Compose blueprint |
| **[`n8n_topic_14_requirement_document.pdf`](./n8n_topic_14_requirement_document.pdf)** | Formatted, print-ready requirement documentation PDF |
| **[`Topic_14_Implementation_Plan.md`](./Topic_14_Implementation_Plan.md)** | Step-by-step practical build guide and node blueprints |
| **[`Topic_14_Implementation_Plan.pdf`](./Topic_14_Implementation_Plan.pdf)** | Official assessment reference document |

---

## 🏗️ Master Workflow Architecture
```text
[ N14-01: Manual_Trigger ]
            |
            v
[ N14-02: Code_PrepareOrderPayload ] (Order: State NY, Subtotal $1,200.00)
            |
            v
[ N14-03: Execute_TaxSubWorkflow ] ──> Child 1: Tax Engine
            |                           (Applies NY 8.875% -> Tax $106.50, Grand Total $1,306.50)
            v
[ N14-04: Code_MergeTaxTotals ]
            |
            v
[ N14-05: Execute_PaymentSubWorkflow ] ──> Child 2: Payment Gateway
            |                              (Charges $1,306.50 -> Confirmed: ch_sub_998102)
            v
[ N14-06: Code_BuildOrchestratorSummary ] (Master Telemetry Audit Receipt)
```

---

## 🧪 Validation & Test Matrix
1. **Master Orchestrator Execution:**
   - Input: Subtotal `$1,200.00`, State: `NY`.
   - Tax Sub-Workflow: Applies 8.875% rate $\rightarrow$ `taxAmount: $106.50`, `grandTotal: $1,306.50`.
   - Payment Sub-Workflow: Charges `$1,306.50`, returns auth code `ch_sub_998102`.
   - Master Summary: Emits `ORDER_FULFILLED_VIA_MICROSERVICES` confirmation.
2. **Contract Guardrail Enforcement:**
   - Triggering Tax Sub-Workflow with empty payload `{}` throws `CONTRACT_VIOLATION: Missing required parameter 'state'`.

---

## 🚀 How to Import into n8n
1. Download **`Topic 14 - Sub-Workflows & Modular Orchestrator.json`**.
2. Open n8n, click **+ Add Workflow** $\rightarrow$ **Import from File**.
3. Click **Test Workflow**—the workflow executes with 100% green checkmarks using embedded test pinData!
