---
name: value
description: Value investing strategy that orchestrates analysis plugins into investment decisions. This skill should be used when evaluating companies for investment using Buffett, Munger, Graham, Li Lu, Marks, and Klarman principles.
---

# Value Investing Strategy

Orchestrates existing analysis plugins (quality, forensic, DCF, insider, earnings) into a cohesive value investing evaluation. When analysis plugins aren't available, performs self-contained analysis from raw FMP data.

## Philosophy

Value investing is a way of thinking, not a checklist. The strategy layer exists because individual analyses — however rigorous — don't make investment decisions. Someone has to synthesize business quality, number verification, valuation, and risk into a conviction-weighted verdict.

The unique contribution of the strategy layer:
1. **Synthesis** — Connecting quality assessment to valuation to risk
2. **Inversion** — Systematically asking "how could I be wrong?"
3. **Decision framework** — Converting analysis into Buy / Wait / Pass with kill conditions
4. **Investor principles** — Not decorative quotes, but required analytical questions

## Contract

**Inputs:** Ticker symbol
**Outputs:**
- `verdict.md` — Investment verdict with all 5 phases and investor principle answers
- `synthesis.json` — Structured data (decision, conviction, phase scores, kill conditions)

**Files touched:** Writes to `.analysis/TICKER/strategy-value/`

## Required Skills

| Skill | Required | Purpose |
|-------|----------|---------|
| `apex-data-financial:fmp` | Yes | Financial data — always needed as data source |
| `apex-data-financial:finviz` | Yes | Quick fundamentals snapshot and sentiment |
| `apex-analysis-quality:quality` | No | Phase 1 delegation — business quality assessment |
| `apex-analysis-forensic:forensic` | No | Phase 2 delegation — forensic accounting verification |
| `apex-analysis-insider:insider` | No | Phase 2 enrichment — insider & institutional ownership signals |
| `apex-analysis-dcf:dcf` | No | Phase 3 delegation — DCF valuation |
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
.analysis/TICKER/quality/    → Phase 1 delegation available
.analysis/TICKER/forensic/   → Phase 2 delegation available
.analysis/TICKER/insider/    → Phase 2 enrichment available
.analysis/TICKER/dcf/        → Phase 3 delegation available
.analysis/TICKER/earnings/   → Phase 3 enrichment available
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
- [ ] **V7**: Existing analysis outputs from `.analysis/TICKER/` incorporated when available

## When to Use

**USE when:**
- User asks to evaluate a company for investment
- `/apex-strategy-value:evaluate TICKER` command
- Need a Buy / Wait / Pass decision with conviction level
- Want to synthesize existing analysis outputs into a verdict

**DON'T use when:**
- Only fetching data (use `apex-data-financial:fetch`)
- Running a specific analysis (use `apex-analysis-dcf:analyze`, etc.)
- Screening stocks (no ticker-specific evaluation)
- Non-investment research tasks
