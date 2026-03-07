---
name: value
description: Value investing strategy that orchestrates analysis plugins into investment decisions. This skill should be used when evaluating companies for investment using Buffett, Munger, Graham, Li Lu, Marks, and Klarman principles.
---

# Value Investing Strategy

Orchestrates existing analysis plugins (quality, forensic, valuation, sentiment, earnings) into a cohesive value investing evaluation. When analysis plugins aren't available, performs self-contained analysis from raw FMP data.

## Philosophy

Value investing is a way of thinking, not a checklist. The strategy layer exists because individual analyses — however rigorous — don't make investment decisions. Someone has to synthesize business quality, number verification, valuation, and risk into a conviction-weighted verdict.

The unique contribution of the strategy layer:
1. **Synthesis** — Connecting quality assessment to valuation to risk
2. **Inversion** — Systematically asking "how could I be wrong?"
3. **Decision framework** — Converting analysis into Buy / Wait / Pass with kill conditions
4. **Investor principles** — Not decorative quotes, but required analytical questions

## Methodology Selection

Before starting, classify the investment approach to select the right methodology variant.

### Variant Routing

| Strategy | Detection | Variant | Key Differences |
|---|---|---|---|
| Value Investing | Default | `value-workflow.md` | Margin of safety, moat durability, Buffett/Munger/Graham principles |
| Growth Investing | User specifies growth focus, or company is pre-profit/high-growth | `growth-workflow.md` (future) | TAM/SAM, revenue acceleration, Rule of 40, EV/Revenue valuation |
| Income/Dividend | User specifies income focus, or mature high-yield company | `income-workflow.md` (future) | Dividend safety, payout ratio, yield sustainability, DRIP compounding |
| GARP | User specifies GARP approach | `garp-workflow.md` (future) | PEG ratio, growth-at-reasonable-price, blends value + growth metrics |

### How Routing Works

1. Check if user specified a strategy or approach
2. If not specified, assess company characteristics (pre-profit → growth, high-yield mature → income)
3. Match against variant table above
4. If a variant matches → load that variant's workflow INSTEAD of the default

When variant file doesn't exist yet → use default `value-workflow.md` with a note about strategy-specific adjustments needed.

## Contract

**Inputs:** Ticker symbol
**Outputs:**
- `verdict.md` — Investment verdict with all 5 phases and investor principle answers
- `synthesis.json` — Structured data (decision, conviction, phase scores, kill conditions)

**Files touched:** Writes to `.db/pm/value/TICKER/`

## Required Skills

| Skill | Required | Purpose |
|-------|----------|---------|
| `apex-data-financial:fmp` | Yes | Financial data — always needed as data source |
| `apex-data-financial:finviz` | Yes | Quick fundamentals snapshot and sentiment |
| `apex-analysis-quality:quality` | No | Phase 1 delegation — business quality assessment |
| `apex-analysis-forensic:forensic` | No | Phase 2 delegation — forensic accounting verification |
| `apex-analysis-sentiment:sentiment` | No | Phase 2 enrichment — sentiment & institutional ownership signals |
| `apex-analysis-valuation:dcf` | No | Phase 3 delegation — DCF valuation |
| `apex-analysis-earnings:earnings` | No | Phase 3 enrichment — earnings trajectory and revision momentum |

## References

| File | When to Load | Content |
|------|--------------|---------|
| `references/value-workflow.md` | Always | Core 5-phase orchestration workflow |
| `references/investor-principles.md` | Always | Per-investor decision frameworks |
| `references/phase-understand.md` | Always | Phase 1: Business understanding |
| `references/phase-verify.md` | Always | Phase 2: Number verification |
| `references/phase-value.md` | Always | Phase 3: Valuation |
| `references/phase-invert.md` | Always | Phase 4: Inversion (strategy-unique) |
| `references/phase-verdict.md` | Always | Phase 5: Synthesis and decision |

## Plugin Availability Detection

Before starting, check what analysis outputs already exist:

```
.db/analysis/quality/TICKER/            → Phase 1 delegation available
.db/analysis/forensic/TICKER/           → Phase 2 delegation available
.db/analysis/sentiment/TICKER/           → Phase 2 enrichment available
.db/analysis/valuation/TICKER/dcf/      → Phase 3 delegation available
.db/analysis/earnings/TICKER/           → Phase 3 enrichment available
.db/analysis/valuation/TICKER/comps/    → Phase 3 enrichment (peer-based valuation context)
.db/analysis/earnings/TICKER/research/  → Phase 3+4 enrichment (earnings event context)
```

Also check if analysis plugin skills are available in the current environment (they may be installed but not yet run for this ticker).

## Delegation Routing

| Mode | Condition | Behavior |
|------|-----------|----------|
| **Pure synthesis** | All 5 optional analysis outputs exist | Read and synthesize existing outputs, add inversion + verdict |
| **Hybrid** | Some analysis outputs exist | Use available outputs, self-contain missing phases |
| **Invoke plugins** | Analysis plugins installed, no outputs | Invoke analysis plugins first, then synthesize |
| **Self-contained** | No analysis plugins available | Run all phases from raw FMP data using Claude's reasoning |

The mode is per-phase, not global. Phase 1 might use quality plugin output while Phase 3 runs self-contained.

## Quality Checklist

Before finalizing evaluation, verify:

- [ ] **V1**: All 5 phases executed (Understand, Verify, Value, Invert, Verdict)
- [ ] **V2**: Each phase states its source (plugin output vs self-contained analysis)
- [ ] **V3**: Inversion identifies 3+ ways the thesis could be wrong
- [ ] **V4**: Verdict includes conviction level (High/Medium/Low/Pass) with reasoning
- [ ] **V5**: If Buy decision — margin of safety percentage stated
- [ ] **V6**: Kill conditions defined with specific, measurable triggers
- [ ] **V7**: Existing analysis outputs from `.db/analysis/*/TICKER/` incorporated when available

## When to Use

**USE when:**
- User asks to evaluate a company for investment
- `/apex-pm-value:evaluate TICKER` command
- Need a Buy / Wait / Pass decision with conviction level
- Want to synthesize existing analysis outputs into a verdict

**DON'T use when:**
- Only fetching data (use `apex-data-financial:fetch`)
- Running a specific analysis (use `apex-analysis-valuation:dcf`, etc.)
- Screening stocks (no ticker-specific evaluation)
- Non-investment research tasks
