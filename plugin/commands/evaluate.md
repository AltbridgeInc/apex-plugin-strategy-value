---
description: "Run value investing evaluation for a company"
argument-hint: "TICKER"
---

# /apex-pm-value:evaluate

Run a value investing evaluation using the 5-phase methodology (Understand, Verify, Value, Invert, Verdict).

## Usage
/apex-pm-value:evaluate TICKER

## What This Does
1. Parse ticker from `$ARGUMENTS`
2. Fetch required data via `apex-data-financial:financial-fetcher` agent
3. Run evaluation via the `value-strategist` agent using `value` methodology
4. Save results to `.db/pm/value/TICKER/`

## Output
Results in `.db/pm/value/TICKER/`:
- `verdict.md` — Investment verdict with all 5 phases and investor principle answers
- `synthesis.json` — Structured decision data (decision, conviction, kill conditions)
