# AI Coding Session Summary — O2C Graph Intelligence

## Overview

AI tools were used as an **iterative engineering assistant**, not as a code generator.

Primary usage:

* system design exploration
* schema modeling
* prompt engineering
* debugging edge cases
* refining LLM reliability

---

## Tools Used

* Claude (primary reasoning + iteration)
* Cursor (code refinement and inline suggestions)

---

## Workflow Pattern

The development followed an iterative loop:

```
Problem → Prompt → Output → Validation → Debug → Refine Prompt
```

Each major component was built through multiple iterations.

---

## Key Iterations

### 1. Graph Modeling

**Initial Prompt:**

> “Convert ERP tables into a graph model with nodes and edges”

**Issue:**

* AI suggested incorrect joins between deliveries and orders

**Fix:**

* explicitly constrained relationships based on dataset
* avoided creating artificial foreign keys

**Outcome:**

* graph reflects real ERP ambiguity instead of fabricated structure

---

### 2. NL → SQL Translation

**Initial Issue:**

* incorrect JOIN conditions
* hallucinated columns

**Fix Prompt:**

> “Use only the provided schema. Do not assume additional relationships. Only generate valid SQL.”

**Improvement:**

* added PK/FK annotations in schema
* enforced strict JSON output

---

### 3. JSON Output Reliability

**Issue:**

* LLM wrapped output in markdown or extra text

**Fix:**

* enforced strict JSON format in prompt
* implemented regex-based JSON extraction

---

### 4. SQL Execution Safety

**Prompt Insight:**
AI suggested executing generated queries directly.

**Concern:**

* unsafe execution risk

**Fix:**

* added validation layer:

  * only allow SELECT queries
  * reject all others

---

### 5. Guardrails

**Test Prompts:**

* “Write a poem”
* “Explain quantum mechanics”

**Fix:**

* added system-level instruction:

  * restrict to dataset domain only

---

### 6. Incident Detection Logic

Used AI to:

* identify meaningful ERP anomalies
* translate them into SQL detection patterns

Example:

> “Find delivered orders without billing documents”

Converted into:

* LEFT JOIN anomaly detection query

---

## Debugging Approach

AI was used for:

* hypothesis generation
* edge case identification
* query correction

Final validation was always:

* manual verification against dataset
* checking correctness of SQL output

---

## Key Learnings

* AI performs best when **constrained with schema and rules**
* explicit instructions significantly reduce hallucination
* iterative prompting is essential for reliability
* validation layers are required before execution

---

## Summary

AI was used as a **co-pilot for reasoning and iteration**, not as a source of final truth.

The system design, validation layers, and final decisions were intentionally controlled to ensure:

* correctness
* reliability
* traceability
