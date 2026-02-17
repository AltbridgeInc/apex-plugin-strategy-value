# Apex Value Investing Strategy Plugin

Value investing strategy that orchestrates analysis plugins (quality, forensic, insider, DCF, earnings) into cohesive investment decisions. Uses principles from Buffett, Munger, Graham, Li Lu, Marks, and Klarman.

## Installation

### First-time installation

```bash
# Add marketplace (one-time setup)
claude plugin marketplace add AltbridgeInc/apex-plugin-strategy-value --scope project

# Install plugin
claude plugin install apex-strategy-value@apex-plugin-strategy-value --scope project
```

### Update to latest version

```bash
# Update plugin to latest commit
claude plugin update apex-strategy-value@apex-plugin-strategy-value --scope project

# Or reinstall
claude plugin uninstall apex-strategy-value@apex-plugin-strategy-value --scope project
claude plugin install apex-strategy-value@apex-plugin-strategy-value --scope project
```

### Verify installation

```bash
# Check plugin is installed
cat .claude/settings.json | grep apex-strategy-value
```
## Dependencies

This plugin references skills from:
- **apex-data-financial** (`apex-data-financial:fmp`, `apex-data-financial:finviz`) - Financial data (required)
- **apex-analysis-quality** (`apex-analysis-quality:quality`) - Business quality assessment (optional)
- **apex-analysis-forensic** (`apex-analysis-forensic:forensic`) - Forensic accounting (optional)
- **apex-analysis-insider** (`apex-analysis-insider:insider`) - Insider & institutional ownership (optional)
- **apex-analysis-dcf** (`apex-analysis-dcf:dcf`) - DCF valuation (optional)
- **apex-analysis-earnings** (`apex-analysis-earnings:earnings`) - Earnings trajectory analysis (optional)

When analysis plugins are installed, the strategy synthesizes their outputs. When they're not, it performs self-contained analysis from raw FMP data.

## Skills

**value** - Value investing strategy methodology including:
- 5-phase evaluation: Understand, Verify, Value, Invert, Verdict
- Investor principle frameworks (Buffett, Munger, Graham, Li Lu, Marks, Klarman)
- Plugin orchestration with graceful fallback to self-contained analysis
- Inversion phase unique to strategy layer

## Agents

**value-strategist** - Executes value investing evaluation for companies
- Orchestrates analysis plugins or runs self-contained analysis
- Executes all 5 phases with investor principle checkpoints
- Produces verdict.md and synthesis.json

## Commands

**`/apex-strategy-value:evaluate TICKER`** - Run value investing evaluation for a company

## Output

Every evaluation produces two outputs in `.analysis/TICKER/strategy-value/`:
- `verdict.md` - Investment verdict with all 5 phases, investor principles, and decision
- `synthesis.json` - Structured data (decision, conviction, scores, kill conditions)

## Repository

https://github.com/AltbridgeInc/apex-plugin-strategy-value

## License

MIT License
