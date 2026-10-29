---
name: arx-tdd-review
description: Vertical-slice TDD at public seams + Orthogonal Two-Axis Diff Review (Axis 1: Standards & 12 Fowler Smells offloaded to Mistral MCP; Axis 2: Spec Fidelity & M4 3-Tier Security Audit on Primary Agent). Use when implementing features test-first or reviewing git diffs/PRs.
---

# ARX Vertical-Slice TDD & Two-Axis Diff Review (`arx-tdd-review`)

Synthesizes Matt Pocock's `tdd` and `code-review` skills with **ARX Kernel v10.1 (`§2.1 Tier 2 Mistral Router`, `[M2]`, `[M4]`)**.

---

## PART 1: VERTICAL-SLICE SEAM TDD

### 1. Seam Declaration (Pre-Implementation)
Before writing tests or production code, declare the **public seam(s)** under test in your Pre-Change Assessment (`§0.4`).
- Read `GLOSSARY.md` (if present) so test descriptions and identifiers use canonical domain vocabulary.
- Tests MUST cross the same public interface as production callers.

### 2. The 3 Banned Testing Anti-Patterns
1. **Horizontal Slicing (Banned):** Never write 10 unit tests upfront before writing implementation code. Bulk pre-written tests lock in imagined interfaces. Work strictly in **vertical tracer-bullet slices**:
   `1 Failing Seam Test (Red) -> 1 Minimal Implementation (Green) -> Repeat`.
2. **Implementation-Coupled Mocking (Banned):** Never mock internal functions/collaborators or assert on private state. Only substitute adapters at genuine external I/O seams (DB, Broker API, Clock, Network).
3. **Tautological Assertions (Banned):** Never compute the expected test value using the same formula/code path as the implementation (`expect(calcMalus(co2)).toBe(rate * co2)`). Expected values MUST be independent ground-truth literals or Regulatory/Spec worked examples.

---

## PART 2: ORTHOGONAL TWO-AXIS CODE REVIEW

A diff can follow every coding standard while implementing the wrong spec (or introducing a race condition), or satisfy the spec while rotting the architecture. **Never merge the two review axes into a single pass.**

### Step 1: Pin the Diff Deterministically (Tier 0)
Capture the commit log and diff against the merge-base (slicing output per `§2.1`):
```powershell
git log <base-ref>..HEAD --oneline | Select-Object -First 25
git diff <base-ref>...HEAD --stat | Select-Object -First 30
```

### Step 2: Axis 1 — Standards & 12 Fowler Smells (Offloaded to Tier 2 `mistral_review_diff`)
Invoke `mistral_review_diff` (or `mistral_analyze_code` on `codestral-latest`) with the diff and instruct it to audit strictly against repo conventions (`GEMINI.md`, `CODING_STANDARDS.md`) and the **12 Fowler Smells Baseline**:
1. *Mysterious Name* (identifier hides intent)
2. *Duplicated Code* (identical logic shape across hunks)
3. *Feature Envy* (function reaches into another module's data more than its own)
4. *Data Clumps* (3+ parameters traveling together that belong in a domain struct)
5. *Primitive Obsession* (raw string/float used instead of a validated domain type, e.g., currency/ticker/VIN)
6. *Repeated Switches* (same branching cascade repeated across files)
7. *Shotgun Surgery* (one logical change scattered across many files)
8. *Divergent Change* (one module mutated for unrelated reasons)
9. *Speculative Generality* (1-adapter abstractions or unused hooks violating the 2-Adapter Rule)
10. *Message Chains* (deep `a.b().c().d()` law-of-Demeter violations)
11. *Middle Man* (shallow pass-through failing the Deletion Test)
12. *Refused Bequest* (adapter/subclass ignoring its interface contract)

### Step 3: Axis 2 — Spec Fidelity & M4 Systemic Audit (Primary Agent)
Independently audit the diff on the Primary Agent for:
1. **Spec Fidelity:**
   - Missing or partial requirements from the prompt/issue/ADR.
   - Unrequested scope creep or silent behavior changes.
2. **M4 Audit Vectors (Mandatory 3-Tier Remediation for any flaw):**
   - *Vector 1 (Security & Trust):* Hard-coded operational parameters (`§2.2`), secret leaks, injection, auth bypass.
   - *Vector 2 (Concurrency & Integrity):* Non-idempotent order/mutation execution, race conditions, float precision drift.
   - *Vector 3 (Cognitive Surface):* Unvalidated LLM output schemas, unbounded loops.
   - *Vector 4 (Resilience):* Missing timeouts, retry storms, single points of failure.

### Step 4: Side-by-Side Aggregation
Report `## Axis 1: Standards & Smells (Mistral Tier 2)` and `## Axis 2: Spec & M4 Audit (Primary)` as separate sections without cross-ranking, followed by a 1-line verdict per axis (`PASS` | `WARN` | `FAIL`).
