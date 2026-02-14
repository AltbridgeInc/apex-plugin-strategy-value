# Phase 5: Synthesis & Decision

Synthesize all 4 phases into a conviction-weighted investment decision. This is where analysis becomes action.

## Phase Score Synthesis

Review each phase and assign a score:

| Phase | Score | Criteria |
|-------|-------|----------|
| 1. Understand | Strong / Adequate / Weak / Disqualifying | Strong: clear moat, high ROIC, simple business. Weak: unclear moat, average ROIC. Disqualifying: can't explain the business. |
| 2. Verify | Strong / Adequate / Weak / Disqualifying | Strong: clean numbers, no red flags. Weak: minor concerns. Disqualifying: FCF/NI divergence, accounting issues. |
| 3. Value | Strong / Adequate / Weak / Disqualifying | Strong: >30% margin of safety. Adequate: 20-30%. Weak: <20%. Disqualifying: overvalued with no catalyst. |
| 4. Invert | Strong / Adequate / Weak | Strong: thesis survives stress testing, clear edge over consensus. Adequate: thesis holds but assumptions are fragile. Weak: thesis depends on everything going right. |

**Any single Disqualifying score → automatic PASS.** Don't rationalize around it.

## Conviction Framework

| Conviction | Requirements |
|------------|-------------|
| **High** | All phases Strong or Adequate. Clear edge over consensus (from Phase 4). Margin of safety >30%. Business you'd be comfortable owning for 10 years. |
| **Medium** | Most phases Strong or Adequate, one Weak. Margin of safety 20-30%. Some uncertainty but thesis is sound. |
| **Low** | Multiple Adequate/Weak phases. Margin of safety exists but thin. Thesis depends on assumptions you can't fully verify. |
| **Pass** | Any Disqualifying phase. OR conviction is Low with no compelling catalyst. OR inversion reveals thesis is built on hope rather than evidence. |

## Decision Matrix

| Decision | When | Required Conditions |
|----------|------|---------------------|
| **BUY** | Business is good, numbers are clean, price is right, and you've honestly stress-tested the thesis | Conviction High or Medium + margin of safety >20% + at least 3 kill conditions defined |
| **WAIT** | Good business, but price isn't right yet | Phase 1 Strong/Adequate + Phase 2 Strong/Adequate + margin of safety <20% or Phase 3 Weak |
| **PASS** | Any disqualifier, or conviction too low to act | Any Disqualifying phase OR conviction Low with no clear catalyst OR Phase 4 reveals fatal assumptions |

## Position Sizing Guidance

If BUY, suggest position sizing based on conviction:

| Conviction | Suggested Sizing | Rationale |
|------------|-----------------|-----------|
| **High** | Full position (3-5% of portfolio) | High confidence, strong margin of safety |
| **Medium** | Half position (1.5-2.5% of portfolio) | Sound thesis but some uncertainty; leave room to add on weakness |

These are guidelines, not prescriptions. Actual sizing depends on portfolio context, concentration limits, and correlation with existing positions.

## Kill Conditions Summary

Aggregate all kill conditions from Phase 4 into a monitoring checklist. For each:
- State the condition clearly
- State the data source for monitoring (earnings reports, quarterly filings, industry data)
- State the action: sell immediately? Reduce position? Reassess?

## Key Risks Ranked

From Phase 4's risk hierarchy, present the top 3-5 risks:

| Rank | Risk | Category | Impact | Likelihood | Monitoring |
|------|------|----------|--------|------------|------------|
| 1 | ... | Permanent loss / Thesis / Volatility / Opportunity | High/Medium/Low | High/Medium/Low | How to detect |
| 2 | ... | ... | ... | ... | ... |
| 3 | ... | ... | ... | ... | ... |

## Investor Principle Final Checks

Before writing the verdict, answer these final questions:

### Buffett's 10-Year Test
> If the stock market closed for 10 years, would you still buy this business at this price?

### Li Lu's Knowledge Compounding
> Will your understanding of this business deepen over time? Will you be a better investor in this company 5 years from now?

### Klarman's Discipline
> Is this genuinely one of the few truly compelling opportunities available right now, or are you just filling a position? Would you be willing to pass and wait for something better?

**Klarman's hardest lesson:** The discipline to do nothing when nothing should be done is the rarest and most valuable skill in investing. If the answer to these questions creates any doubt, PASS is the correct answer.

## verdict.md Template

Use the template defined in `value-workflow.md` to produce the final verdict document.

## synthesis.json Schema

Use the schema defined in `value-workflow.md` to produce the structured output.

Ensure all fields are populated:
- `decision` must be exactly "BUY", "WAIT", or "PASS"
- `conviction` must be exactly "High", "Medium", "Low", or null (if PASS)
- `margin_of_safety_pct` is 0 if WAIT or PASS
- `kill_conditions` array must have 3+ entries if BUY
- `phases` must have scores and sources for all 4 phases (verdict is the synthesis, not a scored phase)
- `metadata.mode` reflects the actual routing used
