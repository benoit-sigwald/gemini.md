# ARX WORKSPACE INSTRUCTIONS
## G:\My Drive · Production Engineering & Quant Trading Overlay

### 1. SCOPE & INHERITANCE
- This workspace inherits all **ARX GLOBAL RULES v9.1** defined in the global `GEMINI.md`.
- Applies to: `Arx Capital`, `Blackstone`, `Dev`, `Behavioral Profiling`.

### 2. WORKSPACE CONVENTIONS
- **Quant & Trading Stack (`g:\Blackstone` & `g:\My Drive\Arx Capital`):**
  - Institutional risk parameters, bracket stops, and Kelly capital velocity apply.
  - Connect to Capital.com via Blackstone MCP and authorized OCI containers.
- **OCI Microservices & MCP Services:**
  - Remote MCP Gateway: `https://arx-mcp.duckdns.org`
  - Servers: `mistral`, `blackstone`, `einstein`, `omni`, `prisme`, `saul`, `auto`.
- **Context & Token Hygiene:**
  - Always slice files surgically (Rule §8).
  - Execute Tier 0 deterministic tools before LLM inference.
  - Offload code analysis, test generation, and log summarization to Tier 2 (Mistral).

### 3. FAST-PATH EXECUTION PROTOCOL ("ORDRE IMMÉDIAT" · ZERO LATENCY)
- **Trigger:** Whenever the user prompts with "ordre immédiat", "execute now", "short now", "buy now", or explicitly commands an immediate trade execution:
  - **ZERO EXPLORATION RULE:** STRICTLY FORBIDDEN to perform directory lookups, file reads, codebase exploration, or multi-turn assessments.
  - **FIRST-STEP EXECUTION:** In the **VERY FIRST TOOL CALL**, execute the direct fast-path binary on OCI:
    `ssh -i "$env:USERPROFILE\.ssh\oci-work.key" ubuntu@145.241.174.15 'docker exec u6iir99qerjbdxs77vydlipx-20261014T063144 node /app/bin/ordre-direct.js --ticker=<TICKER> --sens=<short|long> --risk=<RISK_PCT> [--sl=<SL>] [--tp=<TP>]'`
  - **IMMEDIATE REPORTING:** Output the deal ID, fill price, broker, and server protections directly. Total execution time MUST remain under 5 seconds.

### 4. WEB REPOSITORY CANONICAL PATHS
- **`arxWeb`:** Canonical location is STRICTLY `G:\My Drive\Dev\arxWeb` (tracked to `benoit-sigwald/arxWeb` on GitHub). Never place or look for `arxWeb` under `Trae` or temporary folders. The automotive arbitrage deals cockpit is permanently at `G:\My Drive\Dev\arxWeb\auto-deals.html`.
