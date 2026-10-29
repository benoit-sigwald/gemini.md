---
name: arx-diagnose-bug
description: 5-phase gated diagnosis loop for hard bugs, concurrency faults, and performance regressions. Enforces a tight (<5s) red-capable feedback loop before hypothesizing, load-bearing minimization, falsifiable predictions, and [DEBUG-xxxx] tagged probes.
---

# ARX Gated Bug & Performance Diagnosis (`arx-diagnose-bug`)

Synthesizes Matt Pocock's `diagnosing-bugs` state machine with **ARX Kernel v10.1 (`Law 1: Compute-First`, `§2.1 Mistral Log Offloading`, `[M4: Systemic Audit]`)**.

> **IRON RULE:** Jumping straight to a code hypothesis or speculative fix before constructing a red-capable reproduction command is **STRICTLY FORBIDDEN**.

---

## Phase 1: Build a Tight, Red-Capable Feedback Loop
Spend disproportionate effort here. Construct **one single command** (a seam test, `curl` probe, CLI fixture script, or replay harness) that drives the buggy code path and fails deterministically on the exact user-reported symptom.
- **Secret Redaction:** Never dump raw tokens/keys into terminal output; replace with `<REDACTED>`.
- ** Command Output Slicing (`§2.1`):** Pipe command outputs to `Select-Object -First 25` / `-Tail 30` or `grep`. If logs exceed 50 lines, offload deduplication to `mistral_summarize_logs` (`mistral-small-latest`).
- **Phase 1 Completion Criterion (Must all be true before Phase 2):**
  - [ ] **Red-Capable:** Goes red on *this exact bug* (not a syntax error or unrelated crash).
  - [ ] **Deterministic:** Same verdict on every run (for race conditions/flakes, loop $50\times$ with concurrency stress until reproduction rate $\ge 50\%$).
  - [ ] **Fast:** Executes in $<5$ seconds.

---

## Phase 2: Reproduce & Minimize to Load-Bearing Core
1. Run the Phase 1 command and capture the exact failure signature.
2. Strip away non-essential inputs, config flags, middleware, and steps **one at a time**, re-running the loop after each cut.
3. **Phase 2 Completion Criterion:** Every remaining element in the reproduction is **load-bearing** (removing any single remaining input makes the loop go green).

---

## Phase 3: Formulate 3–5 Ranked Falsifiable Hypotheses
Never anchor on a single plausible theory. Derive **3 to 5 ranked hypotheses** before mutating application logic.
- Every hypothesis MUST state a **falsifiable prediction**:
  > `H1 (P = 0.60): If <mechanism X> is the root cause, then <probe/change Y> will produce <observable output Z>.`
- Account for **M4 Concurrency & Boundary Vectors**: stale cache, float drift, clock skew, missing `await`/lock, container DNS mismatch, or schema coercion.

---

## Phase 4: Targeted Instrumentation (`[DEBUG-xxxx]`)
Test hypotheses in rank order, changing **one variable at a time**:
1. Prefer debugger/REPL evaluation or targeted boundary assertions over log spam.
2. Prefix every temporary log statement with a unique grep tag: `[DEBUG-a4f2]`.
3. For **Performance Regressions**: never guess with logs; capture a baseline timing metric (`performance.now()`, `EXPLAIN ANALYZE`, profiler trace) first, then bisect.

---

## Phase 5: Seam Regression Test, Fix & Cleanup
1. **Lock the Seam First:** Convert the Phase 2 minimal reproduction into a permanent automated test at the public module **seam** (confirming it fails before the fix).
   - *Architectural Finding:* If no public seam exists to catch the bug without mocking internals, report the missing seam as an `[M4 Architectural Hardening]` defect.
2. **Apply Surgical Fix:** Implement the minimal code change that turns the seam test green.
3. **Mandatory Tag Purge:** Run `grep "[DEBUG-"` across the workspace and remove 100% of temporary instrumentation before committing.
