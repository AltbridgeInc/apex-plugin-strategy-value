# Phase 2: Verifying the Numbers

Determine whether the reported financials are trustworthy. Verification is about NOT losing money — you're looking for reasons to walk away, not reasons to invest.

## Delegation Path

**When `.analysis/TICKER/forensic/` exists:**

Read the forensic analysis outputs and extract:
- Beneish M-Score (earnings manipulation probability)
- Altman Z-Score (bankruptcy risk)
- Piotroski F-Score (financial strength)
- Red flags identified
- Overall forensic assessment

Synthesize into Phase 2 summary. Focus on whether the numbers can be trusted for the valuation phase.

**When `apex-analysis-forensic:forensic` plugin is available but no output exists:**

Invoke the forensic plugin for this ticker first, then read the output.

## Self-Contained Path

**When no forensic plugin or output available:**

Use raw FMP data from `.data/financial/fmp/TICKER/` to verify:

### 1. FCF vs Net Income (5Y)
- Compare free cash flow to net income for each of last 5 years
- FCF should track net income over time (not perfectly, but directionally)
- **Red flag:** Net income consistently higher than FCF → earnings may not be real cash
- **Red flag:** Growing gap between the two → accruals building up

### 2. Cash Flow Quality
- Operating cash flow trend — growing, stable, or declining?
- Capex as % of operating cash flow — is the business a cash machine or a capital pit?
- Free cash flow margin trend — the ultimate quality signal

### 3. Debt Trajectory
- Total debt / EBITDA — is leverage manageable? (above 3x = caution, above 5x = alarm)
- Debt trend over 5 years — increasing or decreasing?
- Interest coverage ratio — can they service the debt comfortably?
- Upcoming maturities — any refinancing risk?

### 4. Share Dilution
- Shares outstanding trend (5Y) — is the share count growing?
- Stock-based compensation as % of revenue — excessive dilution?
- **Red flag:** Rising shares + rising debt = management extracting value from shareholders

### 5. Revenue Quality
- Receivables growth vs revenue growth — receivables growing faster = potential channel stuffing
- Deferred revenue trend — growing deferred revenue is positive (prepaid by customers)
- Revenue concentration — is there customer/product concentration risk?

## Investor Principle Hooks

### Munger's "Avoid Stupidity" Check (Required)
> What's the dumbest thing I could do here?

The dumbest thing is usually ignoring red flags because you're excited about the thesis. List any red flags found, even minor ones. Munger's insight: it's more important to avoid being stupid than to be brilliant.

### What You Don't Know (Required)
> What aspects of these financials do I NOT understand?

Be honest. Off-balance-sheet items? Related-party transactions? Complex revenue recognition? Acknowledging gaps is part of verification.

## Phase 2 Output

Produce a summary with:
- **Source:** [plugin output / self-contained]
- FCF vs Net Income alignment (aligned / diverging / red flag)
- Debt trajectory (improving / stable / deteriorating)
- Share dilution (minimal / moderate / aggressive)
- Red flags found (list each, or "none identified")
- Munger's "avoid stupidity" answer
- Gaps in understanding acknowledged
- **Phase 2 Score:** Strong / Adequate / Weak / Disqualifying

**Disqualifying conditions:**
- FCF consistently negative while reporting positive net income
- Debt spiraling with no clear path to deleveraging
- Multiple forensic red flags (M-Score > -1.78 if available)
- Accounting practices you cannot understand or verify
