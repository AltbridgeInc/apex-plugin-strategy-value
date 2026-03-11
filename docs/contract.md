# Plugin Contract: apex-pm-value

## Capabilities
- Value investing evaluation orchestrating analysis plugins into cohesive decisions
- 5-phase methodology: Understand, Verify, Value, Invert, Verdict
- Investor principle frameworks (Buffett, Munger, Graham, Li Lu, Marks, Klarman)
- Self-contained analysis paths when analysis plugins aren't installed
- Inversion and pre-mortem analysis unique to PM layer

## Inputs
- Evaluation request (ticker)
- Financial data from `.db/data/financial/fmp/` (via apex-data-financial plugin)
- Existing analysis outputs from `.db/analysis/*/TICKER/` (when available)

## Outputs
- `verdict.md` — Investment verdict with 5 phases, investor principles, decision
- `synthesis.json` — Structured data (decision, conviction, phase scores, kill conditions)

All outputs written to `.db/pm/value/TICKER/`.

## Data Paths
- Reads from: `.db/data/financial/fmp/`, `.db/analysis/*/TICKER/`
- Writes to: `.db/pm/value/TICKER/`

## Dependencies
- **Plugins (required):** `apex-data-financial` (fmp + finviz)
- **Plugins (optional):** `apex-analysis-quality`, `apex-analysis-forensic`, `apex-analysis-sentiment`, `apex-analysis-valuation`, `apex-analysis-earnings`
- **Runtime:** None (no scripts — PM reasons over data, doesn't compute)
- No direct API keys required (uses data from financial plugin)

## Commands
- `/apex-pm-value:evaluate TICKER` — Run value investing evaluation

## Agent
- `apex-agent-pm-value` — Orchestrates 5-phase value investing evaluation
