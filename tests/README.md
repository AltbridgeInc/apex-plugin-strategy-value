# Testing: apex-pm-value

## Manual Testing

### With Analysis Plugins (Pure Synthesis Mode)

1. Install all analysis plugins: `apex-analysis-quality`, `apex-analysis-forensic`, `apex-analysis-sentiment`, `apex-analysis-valuation`, `apex-analysis-earnings`
2. Run analyses first to populate `.db/analysis/*/TICKER/`
3. Run PM evaluation:
   ```
   /apex-pm-value:evaluate AAPL
   ```
4. Verify `.db/pm/value/AAPL/verdict.md` synthesizes existing outputs
5. Verify `synthesis.json` metadata shows `mode: "pure-synthesis"` or `"hybrid"`

### Without Analysis Plugins (Self-Contained Mode)

1. Ensure only `apex-data-financial` is installed
2. Run:
   ```
   /apex-pm-value:evaluate MSFT
   ```
3. Verify verdict.md has all 5 phases completed via self-contained analysis
4. Verify `synthesis.json` metadata shows `mode: "self-contained"`

### Verification Checklist

- [ ] All 5 phases present in verdict.md
- [ ] Each phase states its source (plugin output vs self-contained)
- [ ] Investor principles answered (not just quoted)
- [ ] Inversion has 3+ ways thesis could be wrong
- [ ] Verdict includes conviction level with reasoning
- [ ] If Buy: margin of safety stated
- [ ] Kill conditions defined with specific triggers
- [ ] synthesis.json is valid JSON with all required fields
