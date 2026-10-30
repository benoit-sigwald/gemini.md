---
name: einstein
description: "Universal Deep Research, Web/Academic Intelligence & Epistemic Triangulation Engine via Einstein MCP (https://arx-mcp.duckdns.org). Trigger with /einstein <query> or when investigating primary sources, validating claims, or conducting due diligence."
---

# `/einstein` — Universal Deep Research & Epistemic Triangulation Engine

Connects Antigravity directly to the **Einstein MCP Server** (`https://arx-mcp.duckdns.org`) on OCI, enforcing **Module [M6: DEEP RESEARCH & EPISTEMIC TRIANGULATION]** and multi-source epistemic rigor.

---

## 1. Universal Entry Protocol

When invoked via `/einstein <query>` or explicit research directive:

1. **Assess Depth Mode from Query Context:**
   - **Standard Mode (`einstein`)**: Factual lookup, background briefing, fast source check.
   - **Weighted Mode (`EinsteinPlus` / `einstein+`)**: Contested claims, valuation, statistical arbitrage, policy/market disputes requiring 7-axis weighting.
   - **Extended Mode (`EinsteinPlusPlus` / `einstein++`)**: High-stakes due diligence, critical legal/financial decisions, deep OSINT, controversial figures or claims.

2. **Call Universal Entry Tool (Tier 1 MCP):**
   - Execute `call_mcp_tool` on server `einstein` with the corresponding tool:
     - Standard: `ToolName: "einstein"`, `Arguments: { "demande": "<query>" }`
     - Weighted: `ToolName: "EinsteinPlus"`, `Arguments: { "demande": "<query>" }`
     - Extended: `ToolName: "EinsteinPlusPlus"`, `Arguments: { "demande": "<query>" }`
   - **Read the Response:** Inspect `archives_pertinentes` (cached arxdb research) and follow the instructions in the `mission` field.

---

## 2. Investigation & Triangulation Workflow

Execute the mission returned by Einstein using the specialized tool suite:

### A. Information Retrieval
- **Web Intelligence (`einstein_web_search`):** Free multi-engine search (DuckDuckGo, Bing/SearxNG fallback, Tavily) with 24h caching. Run bilingual queries (English + French) to cross linguistic bias.
- **Academic Research (`einstein_academic_search`):** Queries Semantic Scholar, arXiv, and OpenAlex for peer-reviewed papers, DOIs, citation counts, and methodology abstracts.
- **Deep Content Extraction (`einstein_fetch_page` / `einstein_deep_fetch`):**
  - Read decisive pages via Jina Reader.
  - Provide `question` argument to let Mistral extract verbatim passages (-80% context tax).

### B. Epistemic Weighing & Proof Hierarchy
- **Source Triangulation (`einstein_triangulate`):**
  - Evaluates proof levels across domains:
    - **Tier 1:** Academic peer-reviewed, statutory text, official regulatory filings (SEC, AMF, Court dockets).
    - **Tier 2:** Established investigative media, audited industry reports, primary technical documentation.
    - **Tier 3:** General web, corporate blogs, commentary.
- **Deterministic 7-Axis Weighting (`einstein_ponderer`):**
  - Evaluates sources across 7 deterministic dimensions: *Level, Independence, Robustness, Freshness, Directness, Traceability, Convergence*.
  - A counter-argument weights down; it does not erase.

### C. Triangulation Criteria
- **Standard (`einstein`):** 1–2 angles · 2–4 targeted sources.
- **Weighted (`EinsteinPlus`):** 3 distinct angles · 6–8 open sources · at least 3 independent measurements of the same phenomenon.
- **Extended (`EinsteinPlusPlus`):** 4–5 angles · 12–15 sources · 5 independent measures · systematic academic & OSINT verification.

---

## 3. Persistence & Output Standards

1. **Archive in OCI ArxDB (`einstein_save_recherche`):**
   - Save the completed structured research to PostgreSQL 18 `ArxDB01` so future sessions have instant zero-token access via `einstein_search_recherches` / `einstein_get_recherche`.

2. **Save Local Markdown Note (When in an active repository):**
   - Save cited summary to `docs/research/YYYY-MM-DD-<slug>.md`.

3. **Report Format:**
   ```markdown
   # Deep Research Report: <Topic>
   - **Mode:** Standard | Einstein+ | Einstein++
   - **Proof Tier Breakdown:** Tier 1: X% | Tier 2: Y% | Tier 3: Z%
   - **Sources Consulted:** <Count> (<List with links>)
   
   ## 1. Executive Synthesis & Empirical Facts [Fact/Theorem]
   <Direct, unhedged findings backed by verifiable primary citations.>
   
   ## 2. Epistemic Divergence & Triangulation
   <Where independent sources agree vs. where methodology/funding biases cause divergence.>
   
   ## 3. Causal Assessment & Risk of Ruin [Logical Deduction]
   <Expected value EV, confidence interval [0.0, 1.0], and top failure modes under Jacobi Inversion.>
   ```
