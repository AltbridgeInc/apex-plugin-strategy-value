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

## Insider Enrichment

Insider signals enrich Phase 2 regardless of whether forensic delegation was used. Management conviction (or lack thereof) is a powerful trust signal — Munger's "show me the incentives."

### Insider Delegation Path

**When `.analysis/TICKER/insider/insider-profile.json` exists:**

Read the insider profile and extract:
- Overall score (0-10) and verdict (Strong Conviction / Positive Signals / Neutral / Distribution / Significant Selling)
- Insider trading patterns: role-weighted buy/sell ratio, material trades, notable buyers
- Routine vs opportunistic classification: opportunistic buy/sell ratio, signal change direction
- Institutional flow: net flow direction (dollars + % of float), new vs closed positions
- Ownership structure: insider %, institutional %, top-10 concentration, holder quality
- Cluster buying alerts (2+ insiders buying within 30 days, each >$100K)
- Flags (any component scoring < 3)

Wire into Phase 2 assessment:
- **Score ≥ 7 (Positive/Strong Conviction):** Insiders are putting money where their mouth is — supports trust in reported numbers
- **Score 4-6.99 (Neutral):** No strong signal either way — weight other verification factors more
- **Score < 4 (Distribution/Significant Selling):** Red flag — heavy insider selling while reporting great numbers is Munger's "avoid stupidity" trigger
- **Cluster buying:** Particularly bullish signal — multiple insiders independently buying suggests conviction
- **Opportunistic sells during earnings beats:** Contradicts reported strength — escalate to red flag

**When `apex-analysis-insider:insider` plugin is available but no output exists:**

Invoke the insider plugin for this ticker first, then read the output.

### Insider Self-Contained Path

**When no insider plugin or output available:**

Use raw FMP data to assess insider conviction:
- Pull insider trading data for last 12 months
- Count buys vs sells by dollar amount (not just transaction count)
- Note any cluster buying pattern (multiple insiders buying within 30 days)
- Check if selling is concentrated in top executives vs broad-based
- Pull institutional ownership percentage for governance signal
- This provides a simplified version of what the full insider plugin does with its 4-component scoring

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

If insider data is available: heavy selling by insiders while reporting great numbers is a classic "avoid stupidity" failure. Insiders know more than you — if they're selling, ask why before you buy.

### What You Don't Know (Required)
> What aspects of these financials do I NOT understand?

Be honest. Off-balance-sheet items? Related-party transactions? Complex revenue recognition? Acknowledging gaps is part of verification.

## Phase 2 Output

Produce a summary with:
- **Source:** [plugin output / self-contained]
- **Insider source:** [insider plugin / self-contained / not available]
- FCF vs Net Income alignment (aligned / diverging / red flag)
- Debt trajectory (improving / stable / deteriorating)
- Share dilution (minimal / moderate / aggressive)
- Insider conviction (strong buying / neutral / distribution / not assessed) — from insider enrichment
- Red flags found (list each, or "none identified")
- Munger's "avoid stupidity" answer
- Gaps in understanding acknowledged
- **Phase 2 Score:** Strong / Adequate / Weak / Disqualifying

**Disqualifying conditions:**
- FCF consistently negative while reporting positive net income
- Debt spiraling with no clear path to deleveraging
- Multiple forensic red flags (M-Score > -1.78 if available)
- Accounting practices you cannot understand or verify
