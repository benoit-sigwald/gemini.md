---
name: arx-deep-architecture
description: Deep-module codebase design and architectural deepening survey (Ousterhout + Feathers + ARX M2/M4). Use when designing module interfaces, choosing seam locations, eliminating shallow pass-throughs, running "Design-It-Twice" interface comparisons, or surveying a codebase for refactoring candidates.
---

# ARX Deep Codebase Architecture (`arx-deep-architecture`)

Synthesizes John Ousterhout (*A Philosophy of Software Design*), Michael Feathers (*Seam Model*), and Matt Pocock's `codebase-design` / `improve-codebase-architecture` with **ARX Kernel v10.1 (`[M2]` & `[M4]`)**.

## 1. Canonical Vocabulary (Strict Usage)

Use these exact terms across specs, reviews, and code:
- **Module:** Any unit with an **Interface** and an **Implementation** (function, class, package, or vertical service slice). *Avoid:* "component", "unit".
- **Interface:** Everything a caller must know to invoke the module safely: type signature, preconditions/invariants, ordering constraints, error modes, latency bounds, and idempotency guarantees.
- **Depth:** Leverage at the interface—the amount of domain behavior and complexity hidden behind a minimal interface surface.
- **Seam:** The exact location where behavior can be altered or verified (e.g., swapping a live broker/API for an in-memory fake) without editing the implementation behind it.
- **Adapter:** A concrete implementation satisfying an Interface at a Seam.
- **Leverage:** Capability gained per unit of interface learned by callers.
- **Locality:** Concentration of bugs, state transitions, and modifications inside a single module rather than scattered across $N$ callers.

---

## 2. Core Design Laws

1. **The Deletion Test:** Imagine deleting the module and inlining it into its callers.
   - If total system complexity *decreases*, the module was a **shallow pass-through** (*Middle Man*)—delete and inline it.
   - If complexity explodes across $N$ callers, the module is **deep** and earns its keep.
2. **The 2-Adapter Rule:**
   - **1 Adapter = Hypothetical Seam (Speculative Generality).** Do not introduce an interface/seam if only one concrete implementation exists and no test/runtime boundary varies across it.
   - **2+ Adapters = Real Seam.** Introduce a seam when isolating external I/O (e.g., `CapitalComBrokerAdapter` vs. `DeterministicSimBrokerAdapter`; `PostgresRepo` vs. `InMemoryRepo`).
3. **Pure Value Core, Imperative Shell:**
   - Push side effects (network, DB, SSH, DOM) to thin outer adapters. Keep pricing, risk sizing, VAT/malus math, and state machines as pure, deterministic value-in/value-out functions.
4. **The Interface Is the Only Test Surface:**
   - Callers and tests cross the exact same seam. If testing a module requires mocking its private helpers or inspecting internal state, the module's interface is the wrong shape.

---

## 3. Workflow A: "Design-It-Twice" (For New Interfaces)

Before implementing a non-trivial module:
1. Formulate **2 radically different interface shapes** (e.g., *Stateful Lifecycle Class* vs. *Pure Functional Pipeline*; *Fine-Grained Command Methods* vs. *Single Declarative Intent Struct*).
2. Compare them in a dense table across 4 metrics:
   - **Interface Surface Area** (number of methods, parameter complexity, temporal coupling)
   - **Hidden Complexity (Depth)** (invariants enforced automatically inside the module)
   - **Seam Testability** (lines of setup needed for a red-green test)
   - **M4 Failure Modes** (race conditions, partial failure states, idempotency leaks)
3. Select the deeper interface and implement immediately.

---

## 4. Workflow B: Codebase Deepening Survey

When asked to audit or improve codebase architecture:
1. **Tier 0 Structural Scan:** Locate high-churn files, pass-through layers, and coupled call chains using targeted `grep` / AST searches (respecting the 150-line read slice cap). Offload bulk file summarization to `mistral_analyze_code` (`codestral-latest`) if scanning $>3$ modules.
2. **Identify Shallow Clusters:** Look for:
   - *Pass-through chains* ($A \rightarrow B \rightarrow C$ with identical parameters)
   - *Shotgun Surgery* (one domain rule split across 4 files)
   - *Temporal coupling* (caller must call `init()`, `validate()`, `execute()` in exact order instead of one deep method)
   - *Leaky seams* (callers constructing raw SQL/HTTP payloads or handling provider-specific retry logic)
3. **Output Candidates Table:** Rank top 3 deepening opportunities by **Leverage ($\Delta$ complexity removed from callers)**, propose the target deep interface, and execute upon confirmation.
