# Apex Value Investing PM Plugin

Value investing PM that orchestrates analysis plugins (quality, forensic, sentiment, valuation, earnings) into cohesive investment decisions. Uses principles from Buffett, Munger, Graham, Li Lu, Marks, and Klarman.

## Installation

### First-time installation

```bash
# Add marketplace (one-time setup)
claude plugin marketplace add AltbridgeInc/apex-plugin-pm-value --scope project

# Install plugin
claude plugin install apex-pm-value@apex-plugin-pm-value --scope project
```

### Update to latest version

```bash
# Update plugin to latest commit
claude plugin update apex-pm-value@apex-plugin-pm-value --scope project

# Or reinstall
claude plugin uninstall apex-pm-value@apex-plugin-pm-value --scope project
claude plugin install apex-pm-value@apex-plugin-pm-value --scope project
```

### Verify installation

```bash
# Check plugin is installed
cat .claude/settings.json | grep apex-pm-value
```

## Dependencies

This plugin references skills from:
- **apex-data-financial** (`apex-data-financial:fmp`, `apex-data-financial:finviz`) - Financial data (required)
- **apex-analysis-quality** — Business quality assessment (optional)
- **apex-analysis-forensic** — Forensic accounting (optional)
- **apex-analysis-sentiment** — Insider & institutional ownership (optional)
- **apex-analysis-valuation** — DCF valuation (optional)
- **apex-analysis-earnings** — Earnings trajectory analysis (optional)

When analysis plugins are installed, the PM synthesizes their outputs. When they're not, it performs self-contained analysis from raw FMP data.

## Skills

**value** - Value investing methodology including:
- 5-phase evaluation: Understand, Verify, Value, Invert, Verdict
- Investor principle frameworks (Buffett, Munger, Graham, Li Lu, Marks, Klarman)
- Plugin orchestration with graceful fallback to self-contained analysis
- Inversion phase unique to PM layer

## Agent

**apex-agent-pm-value** (`apex-pm-value:apex-agent-pm-value`) — Executes value investing evaluation for companies. Orchestrates analysis plugins or runs self-contained analysis. Produces verdict.md and synthesis.json.

## Commands

**`/apex-pm-value:evaluate TICKER`** - Run value investing evaluation for a company

## Output

Every evaluation produces two outputs in `.db/pm/value/TICKER/`:
- `verdict.md` - Investment verdict with all 5 phases, investor principles, and decision
- `synthesis.json` - Structured data (decision, conviction, scores, kill conditions)

## Repository

https://github.com/AltbridgeInc/apex-plugin-pm-value

## License

MIT License
