# Dodge AI — O2C Graph Intelligence

> **AI-powered ERP observability system for Order-to-Cash workflows**
> Graph-based modeling · LLM-driven query engine · Incident diagnostics

---

## Live Demo

**Public Deployment ( netlify):**
[https://your-project-name. netlify.app](https://lucent-scone-29dfeb.netlify.app/)

**Local (zero setup):**
Open `index.html` in any browser

---

## Overview

Enterprise ERP data (orders, deliveries, billing, payments) is inherently fragmented across multiple tables with weak traceability.

This system unifies that data into a **connected graph** and enables:

* Natural language → **structured query generation (SQL)**
* SQL → **execution on real data**
* Results → **graph visualization + insights**
* Built-in **incident detection and diagnosis**

The goal is not just querying — but **debugging and reasoning about ERP flows**.

---

## Core Capabilities

### 1. Graph-Based Data Model

* Nodes:

  * Customers, Sales Orders, Order Items, Deliveries, Billing Documents, Journal Entries, Payments, Products

* Edges:

  * `PLACED_ORDER`, `CONTAINS`, `FULFILLED_BY`, `INVOICED_AS`, `POSTED_TO`, `SETTLED_BY`

**Design Principle:**

> Preserve real ERP relationships and ambiguity instead of forcing artificial joins

---

### 2. Interactive Graph Visualization

* D3 force-directed layout
* Node inspection (metadata panel)
* Relationship tracing across O2C lifecycle
* Dynamic highlighting from query results

---

### 3. LLM-Powered Query Interface

The system converts natural language into executable queries:

```
User Query
   ↓
LLM (NL → SQL)
   ↓
SQL Safety Layer (SELECT-only)
   ↓
Execution (in-browser)
   ↓
Structured response + graph highlighting
```

**Key Property:**

> Every answer is grounded in data — no hallucinated outputs

---

### 4. Incident Detection (Built-in)

The system proactively identifies ERP issues without relying on the LLM:

| Incident                 | Description               | Impact              |
| ------------------------ | ------------------------- | ------------------- |
| Delivered but not billed | Missing billing documents | Revenue leakage     |
| Billed without delivery  | Invalid flow              | Audit risk          |
| Open receivables         | Unsettled journal entries | Cash flow delay     |
| Goods issue not posted   | Delivery blockage         | Pipeline disruption |

Each incident includes:

* Diagnostic SQL
* Affected entities
* Graph highlighting
* Root-cause suggestions

---

## Example: Debugging Revenue Leakage

**Query:**

```
Which orders are delivered but not billed?
```

**Generated SQL:**

```sql
SELECT d.delivery_id, d.so_id
FROM deliveries d
LEFT JOIN billing_documents b ON b.so_id = d.so_id
WHERE d.delivery_status = 'Completed'
AND b.billing_id IS NULL;
```

**System Output:**

* Identifies affected deliveries
* Highlights nodes in the graph

**Root Cause Hypotheses:**

* Billing job failure
* Invoice block in ERP
* Configuration or pricing issue

**Suggested Next Steps:**

* Check billing queue
* Validate invoice block flags
* Review pricing conditions

---

## LLM Integration

### Provider

Uses Hugging Face Inference API (free tier)

---

### Design

A lightweight **LLM Adapter Layer** ensures reliability:

```
User Query
   ↓
LLM Adapter
   ↓
Primary Model
   ↓ (on failure)
Fallback Model
   ↓
JSON Extraction
   ↓
SQL Validation
   ↓
Execution
```

---

### Reliability & Guardrails

* Strict JSON output enforcement
* Schema-aware prompting
* SQL validation (SELECT-only)
* Retry on parsing failure
* Fallback model support
* Domain-restricted responses

Example rejection:

> “This system is designed to answer questions related to the provided dataset only.”

---

## API Configuration

The system uses a free-tier LLM API.

* A **temporary demo key is preloaded in the client** for evaluation convenience
* Users can override it via the UI if needed

**Why this approach:**

* zero setup friction for evaluation
* demonstrates real API usage
* keeps system deployable without backend

**Production approach:**

* API keys stored server-side
* requests proxied via backend
* caching + rate limiting applied

---

## Architecture

### Demo Architecture

```
Browser ( netlify)
 ├── Graph Engine (D3.js)
 ├── SQL Engine (AlaSQL)
 ├── Embedded Dataset
 └── LLM API (client-side)
```

---

### Production Evolution

```
Frontend (React)
    ↓
Backend (FastAPI / Node)
    ├── LLM Adapter Layer
    ├── Query Validation
    ├── Cache (Redis)
    └── Database (PostgreSQL / DuckDB)
```

---

## Why a Single-File Architecture?

This system is intentionally implemented as a single `index.html`.

### Rationale

* **Zero deployment friction** — runs instantly
* **Focus on system design** — not framework overhead
* **Reliability** — no build or dependency failures
* **Full transparency** — entire system visible in one file

### Tradeoffs

* API key is client-side (demo only)
* No backend orchestration
* Limited scalability

---

## Observability

The system tracks:

* LLM latency
* provider usage
* SQL execution logs
* failure cases

This mirrors real-world debugging workflows.

---

## Tech Stack

| Layer         | Technology   |
| ------------- | ------------ |
| Visualization | D3.js        |
| Query Engine  | AlaSQL       |
| LLM API       | Hugging Face |
| UI            | Vanilla JS   |
| Deployment    |  netlify     |

---

## Evaluation Alignment

| Requirement        | Implementation                 |
| ------------------ | ------------------------------ |
| Graph modeling     | Typed nodes + relationships    |
| Visualization      | Interactive graph              |
| NL query interface | LLM → SQL pipeline             |
| Data grounding     | SQL execution                  |
| Guardrails         | Prompt + validation layer      |
| AI usage           | Structured prompting + retries |

---

## What Differentiates This System

Most implementations:

* generate answers

This system:

* executes queries
* shows SQL
* visualizes relationships
* explains root causes

---

## Future Enhancements

* streaming responses
* semantic search
* conversation memory
* graph clustering
* backend API layer

---

## Conclusion

This project demonstrates how AI can be integrated into ERP systems to:

* reduce manual debugging effort
* improve data observability
* accelerate issue resolution

---

**Built for Forward Deployed Engineer evaluation — focused on real-world system design, not just features.**
