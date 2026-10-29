---
name: code-review
description: "Review the changes since a fixed point (commit, branch, tag, or merge-base) along two axes: Standards (repo standards + 12 Fowler smells offloaded to Tier 2 mistral_review_diff) and Spec + M4 Security Audit (Primary Agent). Use when the user wants to review a branch, a PR, work-in-progress changes, or asks to \"review since X\"."
---

Two-axis review of the diff between `HEAD` and a fixed point the user supplies:

- **Axis 1 — Standards & Smells (Offloaded to Tier 2 `mistral_review_diff`):** Does the code conform to this repo's documented coding standards and the 12 Fowler Smells baseline?
- **Axis 2 — Spec Fidelity & M4 Systemic Audit (Primary Agent):** Does the code faithfully implement the originating spec (`docs/specs/NNNN-slug.md` or GitHub Issue) and pass the 4 M4 Security/Concurrency Audit Vectors?

Both axes are evaluated independently and reported side by side so neither masks the other.

## Process

### 1. Pin the fixed point

Whatever the user said is the fixed point (a commit SHA, branch name, tag, `main`, `HEAD~5`, etc.). If they didn't specify one, default to `HEAD~1` (or `main` on a feature branch).

Capture the diff command once: `git diff <fixed-point>...HEAD` (three-dot, so the comparison is against the merge-base). Also note the list of commits via `git log <fixed-point>..HEAD --oneline`.

Before going further, confirm the fixed point resolves (`git rev-parse <fixed-point>`) and the diff is non-empty.

### 2. Identify the spec source (ARX Dual-Write Convention)

Look for the originating spec in this order (never ask the user to run `/setup-matt-pocock-skills`):

1. A spec file under `docs/specs/NNNN-*.md` (or `specs/` / `docs/adr/`) matching the feature or recent commits.
2. Issue references in the commit messages (`#123`, `Closes #45`), fetched via `gh issue view <id>`.
3. A path or prompt requirement the user passed directly in the conversation.
4. If no formal spec file exists, evaluate Axis 2 against the user's conversation prompt and the 4 M4 Audit Vectors.

### 3. Identify the standards sources & Fowler Smell Baseline

Search the repo for documented standards (`GEMINI.md`, `CODING_STANDARDS.md`, `CONTRIBUTING.md`, `GLOSSARY.md`).

On top of whatever the repo documents, the Standards axis always carries the **smell baseline** below: a fixed set of 12 Fowler code smells (_Refactoring_, ch.3) that applies even when a repo documents nothing:

- **Mysterious Name**: a function, variable, or type whose name doesn't reveal what it does or holds (or violates `GLOSSARY.md`). → rename it; if no honest name comes, the design's murky.
- **Duplicated Code**: the same logic shape appears in more than one hunk or file in the change. → extract the shared shape, call it from both.
- **Feature Envy**: a method that reaches into another object's data more than its own. → move the method onto the data it envies.
- **Data Clumps**: the same few fields or params keep travelling together (a type wanting to be born). → bundle them into one type, pass that.
- **Primitive Obsession**: a primitive or string standing in for a domain concept that deserves its own type. → give the concept its own small type.
- **Repeated Switches**: the same `switch`/`if`-cascade on the same type recurs across the change. → replace with polymorphism, or one map both sites share.
- **Shotgun Surgery**: one logical change forces scattered edits across many files in the diff. → gather what changes together into one module.
- **Divergent Change**: one file or module is edited for several unrelated reasons. → split so each module changes for one reason.
- **Speculative Generality**: abstraction, parameters, or hooks added for needs the spec doesn't have (violating the 2-Adapter Rule). → delete it; inline back until a real need shows.
- **Message Chains**: long `a.b().c().d()` navigation the caller shouldn't depend on. → hide the walk behind one method on the first object.
- **Middle Man**: a class or function that mostly just delegates onward (failing the Deletion Test). → cut it, call the real target direct.
- **Refused Bequest**: a subclass or implementer that ignores or overrides most of what it inherits. → drop the inheritance, use composition.

### 4. Execute Both Axes (MCP-First Hybrid Routing)

1. **Axis 1 (Tier 2 `mistral_review_diff` on `codestral-latest`):** Pass the diff and instruct it to report: (a) every violation of documented repo standards (`GEMINI.md` / `GLOSSARY.md`), and (b) any of the 12 Fowler baseline smells spotted, distinguishing hard violations from judgement calls (<400 words).
2. **Axis 2 (Primary Agent — Spec + M4 4-Vector Audit):** Independently report: (a) missing or partial spec requirements; (b) unrequested scope creep; (c) implementation flaws across the **4 M4 Audit Vectors** (*1. Trust Boundaries & Zero Hard-Coded Params*, *2. Concurrency & Idempotency*, *3. Cognitive Attack Surface*, *4. Resilience/SPOF*), outputting the mandatory **3-Tier Remediation** (`[Vector & Severity]`, `[Immediate Hotfix]`, `[Architectural Hardening]`) for any flaw found.

### 5. Aggregate Side by Side

Present the two reports under `## Axis 1: Standards & Smells (Mistral Tier 2)` and `## Axis 2: Spec & M4 Audit (Primary)` headings without merging or cross-ranking findings.

End with a one-line summary: total findings per axis, and the verdict (`PASS` | `WARN` | `FAIL`) _within each axis_.
