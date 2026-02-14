# Plugin Contract: apex-strategy-value

## Capabilities
- Value investing evaluation orchestrating analysis plugins into cohesive decisions
- 5-phase methodology: Understand, Verify, Value, Invert, Verdict
- Investor principle frameworks (Buffett, Munger, Graham, Li Lu, Marks, Klarman)
- Self-contained analysis paths when analysis plugins aren't installed
- Inversion and pre-mortem analysis unique to strategy layer

## Inputs
- Evaluation request (ticker)
- Financial data from `.data/financial/fmp/*` (via apex-data-financial plugin)
- Existing analysis outputs from `.analysis/TICKER/{quality,forensic,insider,dcf,earnings}/` (when available)

## Outputs
- `verdict.md` — Investment verdict with 5 phases, investor principles, decision
- `synthesis.json` — Structured data (decision, conviction, phase scores, kill conditions)

All outputs written to `.analysis/TICKER/strategy-value/`.

## Data Paths
- Reads from: `.data/financial/fmp/*`, `.analysis/TICKER/{quality,forensic,insider,dcf,earnings}/`
- Writes to: `.analysis/TICKER/strategy-value/`

## Dependencies
- **Plugins (required):** `apex-data-financial` (fmp + finviz)
- **Plugins (optional):** `apex-analysis-quality`, `apex-analysis-forensic`, `apex-analysis-insider`, `apex-analysis-dcf`, `apex-analysis-earnings`
- **Runtime:** None (no scripts — strategy reasons over data, doesn't compute)
- No direct API keys required (uses data from financial plugin)

## Commands
- `/apex-strategy-value:evaluate TICKER` — Run value investing evaluation

## Agents
- `value-strategist` — Orchestrates 5-phase value investing evaluation
