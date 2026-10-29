---
name: arx-grill-docs
description: Relentless Compute-First design interview + inline Ubiquitous Language (GLOSSARY.md) & 3-Gate ADR authoring (docs/adr/). Use when stress-testing a plan, spec, or architecture, resolving design tree branches, sharpening domain terminology, or documenting architectural decisions.
---

# ARX Grill & Domain Modeling (`arx-grill-docs`)

Synthesizes Matt Pocock's `grill-with-docs` and `domain-modeling` disciplines with **ARX Kernel v10.1 (`Law 1: Compute-First`, `Law 10: Quantitative Standard`, `Law 12: Jacobi Inversion`)**.

## 1. Execution Protocol: The Design Tree Interview

1. **Locate Existing Domain Context (Tier 0 First):**
   - Check for `GLOSSARY.md`, `GLOSSARY-MAP.md`, and `docs/adr/` at the repo root before asking questions.
   - Cross-reference the user's stated claims against actual codebase seams using targeted `grep`/file slices ($\le 150$ lines).
2. **Jacobi Inversion & Edge-Case Probing:**
   - Identify the unresolved branches of the design tree and the top 3 real-world stress failure modes.
   - Use the `ask_question` tool (with recommended options first, deriving premises before verdicts) to force revealed-preference choices on one branch cluster at a time.
   - Invent concrete boundary scenarios that force sharp distinctions between overloaded concepts (e.g., *Order* vs. *Position* vs. *Execution Fill*; *Margin VAT* vs. *Recoverable VAT*).
3. **Completion Criterion:**
   - The grilling loop terminates ONLY when every branch of the design tree has a deterministic behavioral contract, explicit test seam, and failure-mode mitigation.

---

## 2. Ubiquitous Language Discipline (`GLOSSARY.md`)

- **Challenge & Sharpen Inline:** Whenever the user uses a term that contradicts `GLOSSARY.md` or collapses two distinct domain concepts into one vague word, surface the conflict immediately and propose a canonical term.
- **Update Immediately (Never Batch):** As soon as a domain term crystallizes, write or update `GLOSSARY.md` at the repo root.
- **Zero Implementation Contamination:** `GLOSSARY.md` defines **pure domain concepts, invariants, and relationships**—never database column types, framework classes, or file paths.
- **Format:**
  ```markdown
  # Domain Glossary

  ## Core Concepts
  - **<Canonical Term>**: <1-2 sentence precise domain definition and boundary invariant>. *Avoid:* `<ambiguous synonyms>`.
  ```

---

## 3. High-Bar Architectural Decision Records (`docs/adr/NNNN-slug.md`)

Offer or create an ADR **ONLY when all 3 gates are satisfied**:
1. **Hard to Reverse:** Reversing the decision later carries material engineering, financial, or migration cost.
2. **Surprising Without Context:** A future engineer or agent reading the code would ask *"Why was it built this way?"*
3. **Genuine Trade-Off:** At least two viable alternatives existed with asymmetric Expected Value ($EV$) or failure modes.

### ADR Format (`docs/adr/NNNN-slug.md`)
```markdown
# NNNN. <Decision Title>
- **Date:** YYYY-MM-DD
- **Status:** Accepted | Superseded by NNNN
- **Quantitative Basis:** EV: <X> | CI: [<0.0-1.0>] | P(fail): <Y>

## Context & Forces
<2-4 sentences on the domain problem, constraints, and why a decision is required.>

## Considered Alternatives
1. **<Option A (Chosen)>:** <Pros / Cons / Seam impact>
2. **<Option B (Rejected)>:** <Why rejected>

## Decision & Invariants
<Exact architectural rule and the public seam/interface it governs.>
```
