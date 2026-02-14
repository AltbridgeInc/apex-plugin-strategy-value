---
description: "Run value investing evaluation for a company"
argument-hint: "TICKER"
---

# /apex-strategy-value:evaluate

Run a value investing evaluation for a company using the 5-phase methodology (Understand, Verify, Value, Invert, Verdict) with principles from Buffett, Munger, Graham, Li Lu, Marks, and Klarman.

## Usage

```
/apex-strategy-value:evaluate AAPL
/apex-strategy-value:evaluate MSFT
/apex-strategy-value:evaluate BRK-B
```

## Workflow

### Step 1: Parse Ticker

Extract ticker from `$ARGUMENTS`. Convert to uppercase.

### Step 2: Check Existing Analysis Outputs

Check for existing analysis outputs that can be synthesized:
- `.analysis/TICKER/quality/` — business quality assessment
- `.analysis/TICKER/forensic/` — forensic accounting scores
- `.analysis/TICKER/insider/` — insider & institutional ownership signals
- `.analysis/TICKER/dcf/` — DCF valuation model
- `.analysis/TICKER/earnings/` — earnings trajectory and revision momentum

If outputs exist, the strategy will synthesize them. If not, it checks for installed analysis plugins or falls back to self-contained analysis.

### Step 3: Ensure Financial Data Available

Use `apex-data-financial:financial-fetcher` agent (plugin) to get:
- Company profile
- Financial statements (5 years)
- Key metrics and ratios
- Current quote
- Finviz fundamentals snapshot

### Step 4: Run Evaluation

Launch the `value-strategist` agent which will:
1. Load value investing methodology from `value` skill
2. Assess available resources (existing outputs, installed plugins)
3. Execute 5 phases (Understand, Verify, Value, Invert, Verdict)
4. Answer all investor principle questions
5. Apply V1-V7 quality checklist
6. Write verdict.md and synthesis.json

### Step 5: Save Results

Save to `.analysis/TICKER/strategy-value/`:
```
.analysis/TICKER/strategy-value/
├── verdict.md        # Investment verdict with 5 phases
└── synthesis.json    # Structured decision data
```

## Output

The `value-strategist` agent produces two outputs:

### `verdict.md` (Investment Verdict)
| Section | Content |
|---------|---------|
| Header | Decision (BUY/WAIT/PASS), conviction, price vs intrinsic value |
| Business in One Sentence | Li Lu's simplicity test |
| Phase 1: Understanding | Moat assessment, ROIC, Buffett's 10Y question |
| Phase 2: Verifying | Number verification, red flags, Munger's check |
| Phase 3: Valuing | Fair value range, margin of safety, Graham + Marks |
| Phase 4: Inversion | Stress tests, pre-mortem, bias check, kill conditions |
| Phase 5: Verdict | Phase scores, decision, conviction, risks |

### `synthesis.json` (Structured Data)
| Field | Content |
|-------|---------|
| `decision` | BUY, WAIT, or PASS |
| `conviction` | High, Medium, Low, or null |
| `intrinsic_value` | Low/mid/high range |
| `margin_of_safety_pct` | Percentage below fair value |
| `phases` | Per-phase scores and sources |
| `kill_conditions` | Specific triggers that invalidate thesis |
| `risks` | Ranked risk list |
| `metadata` | Mode (synthesis/hybrid/self-contained), plugins used |

## Notes

- Strategy layer reasons over data and analysis — it doesn't compute
- When analysis plugins are installed, it synthesizes their outputs
- When they're not, it performs self-contained analysis from FMP data
- Phase 4 (Inversion) is always self-contained — unique to strategy layer
- All investor principle questions are answered, not just quoted
- Results saved to `.analysis/TICKER/strategy-value/`
