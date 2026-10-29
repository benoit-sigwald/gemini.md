---
name: pb1
description: "Master Playbook 1 (/pb1) — Greenfield Creation (New Process, App, or Multi-Agent System). Chains research (einstein), grill-with-docs (grilling + inline GLOSSARY.md & 3-Gate ADRs), codebase-design (Design-It-Twice), Dual-Write to-spec (docs/specs/ + GitHub Issue), vertical-slice tdd, writing-for-agents, and Two-Axis code-review."
---

# `/pb1` — ARX Master Playbook 1: Greenfield Creation (New Process, App, or Multi-Agent System)

Orchestrates the end-to-end creation of a new process, application, or multi-agent system with **Asymmetric Autonomy**: Socratic interrogation up front, followed by 100% autonomous specification, vertical-slice TDD implementation, and Two-Axis verification once the design tree frontier is empty.

---

## Phase 1: Primary-Source Research *(Conditional)*
- **Skill:** [`../research/SKILL.md`](../research/SKILL.md) + **Tier 1 `einstein` / `saul` / `auto` MCP**
- **Trigger:** Execute when external APIs, academic literature, statutory/fiscal rules (`CGI art. 206`, malus), or third-party protocols have unknown invariants.
- **Action:** Query `einstein_deep_fetch` / `einstein_triangulate` / `saul_article_fr` and record cited findings in `docs/research/NNNN-topic.md`.
- **Gate:** All external constraints verified as `[Fact/Theorem]`.

---

## Phase 2: Design-Tree Grilling & Inline Domain Modeling *(Interactive Gate)*
- **Skills:** [`../grill-with-docs/SKILL.md`](../grill-with-docs/SKILL.md) ([`../grilling/SKILL.md`](../grilling/SKILL.md) + [`../domain-modeling/SKILL.md`](../domain-modeling/SKILL.md))
- **Action:**
  1. Map the system as a **design tree** and apply **Jacobi Inversion (`Law 12`)** to identify the top 3 real-world stress failure modes.
  2. Interview the user across the current **frontier** using `ask_question` (recommended option first). Look up codebase/environment facts yourself; only ask the user for decisions.
  3. **Inline Side Effects (Every Round):**
     - Create or update `GLOSSARY.md` (or `GLOSSARY-MAP.md` for multi-context systems) using [`../domain-modeling/GLOSSARY-FORMAT.md`](../domain-modeling/GLOSSARY-FORMAT.md)—pure domain definitions + `_Avoid_` anti-synonyms, zero implementation details.
     - Create sequential ADRs in `docs/adr/NNNN-slug.md` using [`../domain-modeling/ADR-FORMAT.md`](../domain-modeling/ADR-FORMAT.md) whenever all 3 gates hold: **(1) Hard to reverse**, **(2) Surprising without context**, **(3) Genuine trade-off**.
- **Gate (Human Checkpoint):** Do NOT write application code until the design tree frontier is **empty**. Once the frontier is empty, proceed **autonomously** through Phases 3–7 without further pauses.

---

## Phase 3: Deep Module & Seam Architecture (*Design-It-Twice*)
- **Skill:** [`../codebase-design/SKILL.md`](../codebase-design/SKILL.md) + [`../codebase-design/DESIGN-IT-TWICE.md`](../codebase-design/DESIGN-IT-TWICE.md)
- **Action:**
  1. For each core module, compare **2 radically different interface designs** (via `invoke_subagent` for complex multi-module systems, or inline table for bounded services) on **Depth** (leverage at the interface), **Locality**, and **Seam Testability**.
  2. Enforce **The Deletion Test** (zero shallow pass-throughs) and **The 2-Adapter Rule** (introduce an interface seam only where $\ge 2$ adapters vary, e.g., Live External Provider vs. Deterministic In-Memory Fake).
  3. If building a **Multi-Agent System or Skill**, apply [`../writing-for-agents/SKILL.md`](../writing-for-agents/SKILL.md) and [`../writing-for-agents/SKILL-MECHANICS.md`](../writing-for-agents/SKILL-MECHANICS.md): separate **Context Load** (front-loaded context pointers with 1 trigger per branch) from **Disclosed Reference**, use pre-trained **Leading Words**, and define checkable step completion criteria.

---

## Phase 4: Dual-Write Specification (`docs/specs/` + GitHub Issue)
- **Skill:** [`../to-spec/SKILL.md`](../to-spec/SKILL.md)
- **Action:**
  1. Synthesize the resolved design tree (using `GLOSSARY.md` terminology and respecting `docs/adr/`) into a numbered specification: **`docs/specs/NNNN-slug.md`**.
  2. Define the **highest, fewest public test seams** (ideally 1–2 top-level seams per subsystem).
  3. **Dual-Write:** If the repository has a GitHub remote (`git remote get-url origin`), automatically publish the spec via `gh issue create --title "Spec NNNN: <Title>" --body-file docs/specs/NNNN-slug.md --label "ready-for-agent"` (creating the label first if absent).

---

## Phase 5: Vertical-Slice Seam TDD (`Red -> Green`)
- **Skill:** [`../tdd/SKILL.md`](../tdd/SKILL.md) ([`../tdd/tests.md`](../tdd/tests.md) + [`../tdd/mocking.md`](../tdd/mocking.md))
- **Action:** Implement the spec in **vertical tracer-bullet slices** at the pre-agreed public seams:
  - `Slice N: 1 Failing Seam Test (Red) -> 1 Minimal Implementation (Green) -> Verify`.
  - **Strictly Banned:** Horizontal bulk test dumps before code, mocking internal collaborators, or tautological assertions.

---

## Phase 6: Orthogonal Two-Axis Review & M4 Security Audit
- **Skill:** [`../code-review/SKILL.md`](../code-review/SKILL.md) + **ARX `[M4]`**
- **Action:**
  1. **Axis 1 (Standards + 12 Fowler Smells):** Offload the git diff to Tier 2 `mistral_review_diff` (`codestral-latest`).
  2. **Axis 2 (Spec Fidelity + M4 4-Vector Audit):** Verify on the Primary Agent against `docs/specs/NNNN-slug.md` and the 4 M4 Vectors (Trust Boundaries/No Hard-Coded Params, Concurrency/Idempotency, Cognitive Surface, Resilience/SPOF).
  3. Remediate any Axis-1 or Axis-2 defects immediately and re-run seam tests.

---

## Phase 7: Autonomous Commit, Push & Handoff
- **Skill:** [`../handoff/SKILL.md`](../handoff/SKILL.md) *(or [`../teach/SKILL.md`](../teach/SKILL.md) if user requests interactive onboarding)*
- **Action:** Commit all code, `GLOSSARY.md`, `docs/adr/`, and `docs/specs/` to Git, push to origin, and output the compact ARX v10.3 Telemetry Footer.
