---
name: arx-audit-repo
description: Master Playbook 2 — Complete Audit & Deepening of an Existing Tool or Repo. Chains domain-modeling (GLOSSARY.md reverse-engineering), improve-codebase-architecture (Tailwind/Mermaid HTML visual report), Two-Axis code-review (mistral_review_diff + M4 3-Tier Security Audit), and pauses for candidate selection before Replace-Don't-Layer TDD refactoring. Trigger when auditing, reviewing, or refactoring an existing repository or tool.
---

# ARX Master Playbook 2: Complete Repository Audit & Deepening (`arx-audit-repo`)

Executes a comprehensive architectural, domain, code-smell, and security/concurrency audit on an existing repository, generating a visual HTML report and M4 remediation matrix, then **pausing for candidate selection** before mutating production logic.

---

## Phase 1: Domain Model & Ubiquitous Language Audit
- **Skill:** [`../domain-modeling/SKILL.md`](../domain-modeling/SKILL.md)
- **Action:**
  1. Inspect existing `GLOSSARY.md`, `GLOSSARY-MAP.md`, and `docs/adr/` (or reverse-engineer domain concepts from core schemas/entrypoints using `<150`-line surgical slices).
  2. Identify **terminology drift** where the same domain entity has multiple conflicting names across DB tables, backend modules, and UI components, or where a single overloaded word masks two distinct concepts.
  3. Draft or update `GLOSSARY.md` (with `_Avoid_` anti-synonyms) so all subsequent audit findings use canonical domain vocabulary.

---

## Phase 2: Architectural Deepening Survey & Visual HTML Report
- **Skills:** [`../improve-codebase-architecture/SKILL.md`](../improve-codebase-architecture/SKILL.md) ([`../improve-codebase-architecture/HTML-REPORT.md`](../improve-codebase-architecture/HTML-REPORT.md)) + [`../codebase-design/SKILL.md`](../codebase-design/SKILL.md)
- **Action:**
  1. **Hot-Spot Targeting:** Run `git log --oneline -n 50` (slicing output per `§2.1`) to locate high-churn files and coupled modules.
  2. **Subagent / MCP Codebase Walk:** Dispatch a `research` subagent (`invoke_subagent`) for multi-directory structural mapping and offload file-level complexity triage to Tier 2 `mistral_analyze_code` (`codestral-latest`).
  3. Evaluate modules using [`../codebase-design/SKILL.md`](../codebase-design/SKILL.md) vocabulary (**module**, **interface**, **depth**, **seam**, **adapter**, **leverage**, **locality**):
     - Apply **The Deletion Test** to flag shallow pass-throughs (*Middle Man*).
     - Flag 1-adapter speculative interfaces violating **The 2-Adapter Rule**.
     - Flag scattered logic violating **Locality** (*Shotgun Surgery*).
     - Flag untestable modules lacking a clean external **Seam**.
  4. **Generate & Open Visual HTML Report:** Write a self-contained Tailwind + Mermaid HTML report to `$env:TEMP\architecture-review-<timestamp>.html` following [`../improve-codebase-architecture/HTML-REPORT.md`](../improve-codebase-architecture/HTML-REPORT.md) (side-by-side Before/After diagrams, `Strong` / `Worth exploring` / `Speculative` badges, and Top Recommendation) and launch it via `Start-Process`.

---

## Phase 3: Orthogonal Two-Axis & M4 Systemic Security Audit
- **Skill:** [`../code-review/SKILL.md`](../code-review/SKILL.md) + **ARX Kernel `[M4]`**
- **Action:**
  1. **Axis 1 — Standards & 12 Fowler Smells (Tier 2 `mistral_review_diff` / `mistral_analyze_code`):** Audit hot-spot modules against repo conventions and the 12 Fowler Smells (*Mysterious Name, Duplicated Code, Feature Envy, Data Clumps, Primitive Obsession, Repeated Switches, Shotgun Surgery, Divergent Change, Speculative Generality, Message Chains, Middle Man, Refused Bequest*).
  2. **Axis 2 — M4 4-Vector Systemic Audit (Primary Agent):**
     - *Vector 1 (Trust Boundaries & Config):* Hard-coded URLs/IPs/container IDs (`§2.2`), secret exposure, auth/IDOR flaws, injection.
     - *Vector 2 (Logical Integrity & Concurrency):* Race conditions, missing idempotency keys (double order/mutation execution), float precision drift, stale state.
     - *Vector 3 (Cognitive Attack Surface):* Prompt/RAG injection, unvalidated LLM schemas, unbounded agent loops.
     - *Vector 4 (Resilience & Chaos):* Single Points of Failure (SPOF), missing circuit breakers/timeouts, retry storms.
  3. Output the **Mandatory 3-Tier Remediation Matrix** for every flaw:
     - `[Vector & Severity]` $\rightarrow$ `[Immediate Hotfix]` $\rightarrow$ `[Architectural Hardening]`.

---

## Phase 4: Human Checkpoint — Candidate Selection & Grilling Gate *(PAUSE)*
- **Skills:** [`../grilling/SKILL.md`](../grilling/SKILL.md) + [`../codebase-design/DEEPENING.md`](../codebase-design/DEEPENING.md) + [`../codebase-design/DESIGN-IT-TWICE.md`](../codebase-design/DESIGN-IT-TWICE.md)
- **Action:**
  1. Present the HTML report path and the M4 3-Tier Matrix, then **PAUSE** using `ask_question` so the user selects which deepening candidates and/or M4 remediations to execute.
  2. If the user rejects a candidate for a load-bearing architectural reason, offer to record a 3-Gate ADR in `docs/adr/NNNN-slug.md` so future audits never re-suggest it.
  3. For the selected candidate(s), run **Design-It-Twice** to lock the deepened target interface and seam.

---

## Phase 5: Replace-Don't-Layer Seam TDD Refactoring
- **Skills:** [`../tdd/SKILL.md`](../tdd/SKILL.md) + [`../codebase-design/DEEPENING.md`](../codebase-design/DEEPENING.md)
- **Action:**
  1. Write seam-level tests at the new deepened module interface first.
  2. Collapse the shallow modules behind the new interface (**Replace-Don't-Layer**—do not leave old pass-through wrappers around).
  3. Delete obsolete shallow internal tests, verify all seam tests pass, and commit.
