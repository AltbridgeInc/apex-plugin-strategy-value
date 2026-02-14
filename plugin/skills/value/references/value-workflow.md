# Value Investing Workflow

The core 5-phase orchestration for evaluating a company as a value investment.

## Step 0: Assess Available Resources

Before starting any phase, determine what's available:

1. **Check existing analysis outputs:**
   - Look for `.analysis/TICKER/quality/` → quality assessment available
   - Look for `.analysis/TICKER/forensic/` → forensic scores available
   - Look for `.analysis/TICKER/dcf/` → DCF valuation available

2. **Check installed plugins:**
   - Is `apex-analysis-quality:quality` skill available? → can invoke for Phase 1
   - Is `apex-analysis-forensic:forensic` skill available? → can invoke for Phase 2
   - Is `apex-analysis-dcf:dcf` skill available? → can invoke for Phase 3

3. **Determine per-phase mode:**
   - Output exists → read and synthesize (delegation)
   - Plugin installed, no output → invoke plugin first (invoke)
   - Neither → self-contained analysis from FMP data

4. **Always ensure FMP data is available:**
   - Use `apex-data-financial:fmp` to fetch data if `.data/financial/fmp/TICKER/` doesn't exist

## Step 1: Phase 1 — Understand the Business

Load `references/phase-understand.md`.

Execute via delegation or self-contained path. Answer:
- What does this business do? (Li Lu's simplicity test)
- What is the moat? Will it be stronger in 10 years? (Buffett's question)
- Is this within my circle of competence?
- ROIC trajectory — is value being created?

**Output:** Business understanding summary with moat assessment and quality signals.

## Step 2: Phase 2 — Verify the Numbers

Load `references/phase-verify.md`.

Execute via delegation or self-contained path. Answer:
- Are the reported numbers trustworthy? (Munger's "avoid stupidity")
- FCF vs Net Income — are earnings real?
- Debt trajectory — is leverage increasing?
- Share dilution — is management enriching itself?

**Output:** Verification summary with red flags (or clean bill of health).

## Step 3: Phase 3 — Determine Value

Load `references/phase-value.md`.

Execute via delegation or self-contained path. Answer:
- What is intrinsic value? What range? (Graham's margin of safety)
- What is the market pricing in? What am I seeing differently? (Marks' second-level thinking)
- Earnings yield vs treasury rate — am I being compensated for equity risk?

**Output:** Valuation summary with fair value range and margin of safety.

## Step 4: Phase 4 — Invert

Load `references/phase-invert.md`.

Always self-contained — this is the strategy layer's unique contribution. No delegation.

- List 3-5 key assumptions and stress-test each
- Pre-mortem: "3 years later, lost 50% — what happened?"
- My view vs market's view — where is the disagreement?
- Psychological bias check (Munger's checklist)
- Define kill conditions

**Output:** Inversion analysis with assumption stress tests, pre-mortem, and kill conditions.

## Step 5: Phase 5 — Verdict

Load `references/phase-verdict.md`.

Synthesize all phases into a decision.

- Score each phase (Strong / Adequate / Weak / Disqualifying)
- Apply conviction framework (High / Medium / Low / Pass)
- Make decision: Buy / Wait / Pass
- If Buy: state margin of safety and position sizing guidance
- Summarize kill conditions and key risks

**Output:** Final verdict with decision, conviction, and kill conditions.

## Output Generation

After all 5 phases, produce two files in `.analysis/TICKER/strategy-value/`:

### verdict.md

```markdown
# Value Investing Verdict: TICKER

**Decision: [BUY / WAIT / PASS]** | **Conviction: [High / Medium / Low]**
**Date:** YYYY-MM-DD
**Current Price:** $XXX | **Intrinsic Value Range:** $XXX–$XXX

---

## The Business in One Sentence
[Li Lu's simplicity test — if you can't explain it simply, you don't understand it]

## Phase 1: Understanding the Business
**Source:** [plugin output / self-contained]
[Business summary, moat assessment, circle of competence]
> **Buffett's 10-Year Question:** Will this competitive advantage be stronger in 10 years?
> [Answer — required, not optional]

## Phase 2: Verifying the Numbers
**Source:** [plugin output / self-contained]
[Verification summary, red flags or clean bill of health]
> **Munger's Check:** What's the dumbest thing I could do here?
> [Answer — required, not optional]

## Phase 3: Determining Value
**Source:** [plugin output / self-contained]
[Valuation summary, fair value range, margin of safety]
> **Graham's Margin of Safety:** What am I paying vs what it's worth?
> [Answer — required, not optional]

## Phase 4: Inversion
**Source:** self-contained (always)
[Assumption stress tests, pre-mortem, bias check]
> **Marks' Second-Level Thinking:** What is consensus, and why might it be wrong?
> [Answer — required, not optional]

## Phase 5: Verdict

### Phase Scores
| Phase | Score | Key Finding |
|-------|-------|-------------|
| 1. Understand | [Strong/Adequate/Weak] | [one line] |
| 2. Verify | [Strong/Adequate/Weak] | [one line] |
| 3. Value | [Strong/Adequate/Weak] | [one line] |
| 4. Invert | [Strong/Adequate/Weak] | [one line] |

### Decision
[BUY / WAIT / PASS] — [reasoning in 2-3 sentences]

### Margin of Safety (if Buy)
[X% below intrinsic value midpoint]

### Kill Conditions
1. [Specific, measurable event that invalidates thesis]
2. [...]
3. [...]

### Key Risks (ranked)
1. [Risk — impact — probability]
2. [...]
3. [...]

> **Klarman's Discipline:** Is this one of the few truly compelling opportunities, or am I stretching?
> [Answer — required, not optional]
```

### synthesis.json

```json
{
  "ticker": "TICKER",
  "date": "YYYY-MM-DD",
  "decision": "BUY | WAIT | PASS",
  "conviction": "High | Medium | Low",
  "current_price": 0.00,
  "intrinsic_value": {
    "low": 0.00,
    "mid": 0.00,
    "high": 0.00
  },
  "margin_of_safety_pct": 0.00,
  "phases": {
    "understand": { "score": "Strong | Adequate | Weak | Disqualifying", "source": "plugin | self-contained", "key_finding": "" },
    "verify": { "score": "...", "source": "...", "key_finding": "" },
    "value": { "score": "...", "source": "...", "key_finding": "" },
    "invert": { "score": "...", "source": "self-contained", "key_finding": "" }
  },
  "kill_conditions": [],
  "risks": [],
  "metadata": {
    "mode": "pure-synthesis | hybrid | invoke | self-contained",
    "plugins_used": [],
    "analysis_outputs_read": []
  }
}
```
