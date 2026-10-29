---
name: arx-debug-app
description: Master Playbook 3 — Autonomous 6-Phase Bug & Performance Regression Diagnosis. Chains arx-diagnose-bug (<5s red-capable loop, load-bearing minimization, 3-5 falsifiable hypotheses, [DEBUG-xxxx] probes + mistral_summarize_logs), domain-modeling check, seam tdd regression test, and Two-Axis code-review. Runs 100% autonomously to a green seam test (pausing only if Phase 1 cannot reproduce).
---

# ARX Master Playbook 3: Autonomous Bug & Performance Diagnosis (`arx-debug-app`)

Executes a strict, phase-gated scientific debugging loop with **Full Autonomous Execution**: once invoked, the agent runs straight through Phases 1–6 to a verified seam regression test and clean diff, **pausing ONLY if Phase 1 genuinely cannot construct a red-capable reproduction loop**.

> **IRON INVARIANT:** Guessing a fix or mutating application logic before Phase 1 produces a deterministic RED command is **STRICTLY FORBIDDEN**.

---

## Phase 1: Construct a Tight (`<5s`), Red-Capable Feedback Loop
- **Skill:** [`../arx-diagnose-bug/SKILL.md`](../arx-diagnose-bug/SKILL.md) (Phase 1)
- **Action:**
  1. Redact all credentials/tokens (`<REDACTED>`) and slice terminal output (`Select-Object -First 25` / `-Tail 30`). If error logs exceed 50 lines, delegate deduplication to Tier 2 `mistral_summarize_logs` (`mistral-small-latest`).
  2. Build **one single automated command** (seam test, `curl` probe, CLI fixture script, Playwright script, trace replay, or $50\times$ concurrency stress harness) that exercises the buggy code path and fails on the **exact user-reported symptom**.
- **Completion Gate:**
  - [ ] **Red-Capable:** Fails specifically on the target bug.
  - [ ] **Deterministic:** Reproduces reliably ($\ge 50\%$ rate for concurrency flakes).
  - [ ] **Fast:** Runs in $<5\text{s}$.
  - *Exception:* Only if environment access or missing production artifacts make local reproduction impossible, **STOP** here and ask the user for the missing artifact/access. Otherwise, proceed immediately to Phase 2.

---

## Phase 2: Reproduce & Minimize to Load-Bearing Core
- **Skill:** [`../arx-diagnose-bug/SKILL.md`](../arx-diagnose-bug/SKILL.md) (Phase 2)
- **Action:** Strip inputs, payload fields, middleware, and config flags **one at a time**, re-running the Phase 1 command after each cut until **every remaining element is load-bearing** (removing any single remaining item turns the loop green).

---

## Phase 3: Domain Check & 3–5 Ranked Falsifiable Hypotheses
- **Skills:** [`../arx-diagnose-bug/SKILL.md`](../arx-diagnose-bug/SKILL.md) (Phase 3) + [`../domain-modeling/SKILL.md`](../domain-modeling/SKILL.md)
- **Action:**
  1. Check `GLOSSARY.md` and `docs/adr/` to confirm the observed behavior is not an intentional domain invariant.
  2. Formulate **3–5 ranked hypotheses** before touching application code. Every hypothesis MUST state a falsifiable prediction:
     > `H1 (P = 0.65): If <root cause X> is responsible, then <probe/change Y> will produce <observable result Z>.`
  3. Explicitly include **M4 Concurrency/Boundary vectors** (race conditions, missing idempotency, float precision drift, stale cache, container DNS/env mismatch).

---

## Phase 4: Targeted Instrumentation (`[DEBUG-xxxx]`) & Falsification
- **Skill:** [`../arx-diagnose-bug/SKILL.md`](../arx-diagnose-bug/SKILL.md) (Phase 4)
- **Action:**
  1. Test hypotheses in rank order, changing **one variable at a time**.
  2. Prefix every temporary diagnostic log or assertion with a unique grep tag: `[DEBUG-a4f2]`.
  3. **Performance Regressions Branch:** Capture a quantitative baseline (`performance.now()`, `EXPLAIN ANALYZE`, profiler trace) before mutating code; bisect to isolate the bottleneck.

---

## Phase 5: Seam Regression Test, Surgical Fix & Tag Purge
- **Skills:** [`../tdd/SKILL.md`](../tdd/SKILL.md) + [`../codebase-design/SKILL.md`](../codebase-design/SKILL.md)
- **Action:**
  1. Convert the Phase 2 minimal reproduction into a permanent automated test at the **public module seam** (verifying it fails before the fix, without internal mocking or tautological assertions).
     - *If no clean seam exists to test the bug without reaching into private internals:* Flag the shallow boundary via `codebase-design` and deepen the seam as part of the fix.
  2. Apply the minimal surgical code fix that turns the seam test **GREEN**.
  3. Run `Select-String -Pattern "\[DEBUG-"` across the repository and remove **100%** of temporary debug probes.

---

## Phase 6: Fast Two-Axis Diff Check
- **Skill:** [`../code-review/SKILL.md`](../code-review/SKILL.md)
- **Action:**
  1. Run Axis-1 (`mistral_review_diff`) and Axis-2 (Spec/M4 check) on the bugfix diff to verify zero Fowler smells and zero unrequested regressions.
  2. Commit and report completion with the ARX Telemetry Footer.
