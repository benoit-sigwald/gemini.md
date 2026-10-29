# ARX WORKSPACE OVERLAY (v10.0)
## G:\My Drive · Production Engineering, Quant Trading & Multi-MCP Operations

### 1. SCOPE, INHERITANCE & MODULE ROUTING
- **Kernel Inheritance:** This workspace inherits all epistemic laws (`§1`), deterministic execution/slicing bounds (`§2`), and composable domain modules (`§3`) from the global [`C:\Users\benoi\.gemini\GEMINI.md`](file:///C:/Users/benoi/.gemini/GEMINI.md) (`ARX SYSTEM KERNEL v10.0`).
- **Default Domain Lens Bindings by Project:**
  - **`G:\Blackstone` & `G:\My Drive\Arx Capital`:** Default `[ACTIVE LENSES: M1, M2, M4]` (Quant Finance + Systems Code + Concurrency/Fault Audit).
  - **`G:\My Drive\Dev\arxWeb` (Automotive Arbitrage Cockpit):** Default `[ACTIVE LENSES: M1, M2, M5]` (Quant Arbitrage + Web/Backend + CGI art. 206 VAT/Malus Tax Engineering).
  - **`Behavioral Profiling` (`Prisme` / `Omni` / `BP v3`):** Default `[ACTIVE LENSES: M3, M6]` (Evolutionary/Neurochemical Profiling + Epistemic Triangulation).
  - **`Legal & Fiscal Engineering` (`Saul`):** Default `[ACTIVE LENSES: M5, M6]` (Statutory/Jurisprudence Rigor + Source Triangulation).

### 2. OCI INFRASTRUCTURE & MCP GATEWAY TOPOLOGY
- **Remote MCP Gateway:** `https://arx-mcp.duckdns.org` (OCI ARM64 · Coolify · Docker · Traefik · PostgreSQL 18 + `pgvector`).
- **Active MCP Servers:**
  - `blackstone`: Live Capital.com order execution, positions, journal, and daily summary.
  - `mistral`: Tier 2 autonomous offloading (`mistral-small-latest`, `codestral-latest`) with 24h LRU cache.
  - `einstein`: Deep web/academic search, page extraction, and epistemic triangulation.
  - `omni` & `prisme`: Behavioral profiling (`BP v3`), PostgreSQL 18 `ArxDB01` observations, numerology/tarot/chart engines, DOCX report linting/rendering.
  - `saul`: French/Swiss/Spanish legal codes, jurisprudence, SPARQL, cadastre, and PLU.
  - `auto`: European automotive deal hunting (AutoScout24, Otomoto, Mobile.de), VIN inspection, VAT normalization, import malus, and business case builder.

### 3. HARDENED FAST-PATH EXECUTION PROTOCOL ("ORDRE IMMÉDIAT" · ZERO LATENCY <5s)
- **Trigger:** Whenever the user prompts with `"ordre immédiat"`, `"execute now"`, `"short now"`, `"buy now"`, or explicitly commands an immediate live trade execution:
  1. **ZERO EXPLORATION & ZERO RITUAL RULE:** STRICTLY FORBIDDEN to perform directory lookups, file reads, codebase exploration, Pre-Change Assessments (`§0.4`), or multi-turn confirmations.
  2. **FIRST-STEP EXECUTION (Dynamic Container Resolution):** In the **VERY FIRST TOOL CALL**, execute the direct fast-path binary on OCI (`145.241.174.15`), resolving the active Coolify container dynamically by service prefix (`u6iir99qerjbdxs77vydlipx`) so container redeployments never break execution:
     ```powershell
     ssh -i "$env:USERPROFILE\.ssh\oci-work.key" ubuntu@145.241.174.15 'CID=$(docker ps --filter "name=u6iir99qerjbdxs77vydlipx" --format "{{.Names}}" | head -n 1) && docker exec "$CID" node /app/bin/ordre-direct.js --ticker=<TICKER> --sens=<short|long> --risk=<RISK_PCT> [--sl=<SL>] [--tp=<TP>]'
     ```
  3. **AUTOMATIC MCP FALLBACK:** If SSH returns non-zero or times out (>3.5s), immediately invoke `blackstone_passer_ordre` on the `blackstone` MCP server in the next tool call.
  4. **IMMEDIATE REPORTING:** Output the Deal ID, fill price, direction, risk %, broker, and server-side SL/TP protections directly. Total execution wall-time MUST remain under 5 seconds.

### 4. CANONICAL REPOSITORY PATHS & WORKSPACE HYGIENE
- **`arxWeb`:** Canonical location is STRICTLY [`G:\My Drive\Dev\arxWeb`](file:///G:/My%20Drive/Dev/arxWeb) (tracked to `benoit-sigwald/arxWeb` on GitHub). Never create or look for `arxWeb` under `Trae` or temporary folders. The automotive arbitrage cockpit is permanently at [`G:\My Drive\Dev\arxWeb\auto-deals.html`](file:///G:/My%20Drive/Dev/arxWeb/auto-deals.html).
- **Surgical Slicing & Offloading Enforcement:** Enforce Global Kernel `§2.1` strictly across all `G:\My Drive` and `G:\Blackstone` repositories (150-line file read cap, 30-line terminal output cap, mandatory Tier 2 `mistral` delegation for log/diff/code summaries).
