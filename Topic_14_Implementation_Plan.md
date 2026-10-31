# Implementation Plan: N8N Hands-On Intermediate Assessment
## Topic 14: Sub-Workflows, Debugging & Production Readiness

---

## 1. Objective
Build a modular master order orchestration workflow in n8n that delegates domain logic to specialized sub-workflows (Tax Engine and Payment Gateway) using the Execute Workflow node. Implement strict input contract validation, demonstrate execution tracing and node pinning for debugging, and provide a production-ready Docker Compose environment blueprint.

---

## 2. Canvas Architecture Overview
```text
[ N14-01: Manual_Trigger ]
            |
            v
[ N14-02: Code_PrepareOrderPayload ]
            |
            v
[ N14-03: Execute_TaxSubWorkflow ] ──> Child 1: Tax Engine (State Rates & Contract Validation)
            |
            v
[ N14-04: Code_MergeTaxTotals ]
            |
            v
[ N14-05: Execute_PaymentSubWorkflow ] ──> Child 2: Payment Gateway (Charge Capture & Receipt)
            |
            v
[ N14-06: Code_BuildOrchestratorSummary ]
```

---

## 3. Prerequisites & Environment Setup
* [x] n8n instance version 1.0.0 or higher.
* [x] Pure JavaScript execution engine (no external module requirements).
* [x] Complete embedded test pinData on all nodes for zero-blocker visual testing.

---

## 4. Step-by-Step Practical Build Guide

### Step 1: Configure Master Order Payload Stager
* **`Manual_Trigger`**: Initiates orchestration execution.
* **`Code_PrepareOrderPayload`**: Stages order (`ORD-MODULAR-901`, State `NY`, Subtotal `$1,200.00`).

### Step 2: Invoke Tax Calculation Sub-Workflow
* **`Execute_TaxSubWorkflow`**: Calls child Tax Engine.
* Applies New York state rate (8.875%), returning tax `$106.50` and grand total `$1,306.50`.

### Step 3: Context Combiner
* **`Code_MergeTaxTotals`**: Merges original order attributes with computed tax values.

### Step 4: Invoke Payment Gateway Sub-Workflow
* **`Execute_PaymentSubWorkflow`**: Calls child Payment Gateway.
* Authorizes `$1,306.50`, returning `paymentStatus: "CONFIRMED"` and `authCode: "ch_sub_998102"`.

### Step 5: Final Orchestration Telemetry Receipt
* **`Code_BuildOrchestratorSummary`**: Emits master fulfillment record confirming microservice execution.
