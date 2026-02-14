# Phase 3: Determining Value

Determine what the business is worth and what you're being asked to pay. The gap between the two is your margin of safety.

## Delegation Path

**When `.analysis/TICKER/dcf/` exists:**

Read the DCF analysis outputs and extract:
- Fair value estimate (base case)
- Bull/bear case range
- Key valuation assumptions (growth rate, WACC, terminal multiple)
- Sensitivity analysis highlights
- Margin of safety at current price

Synthesize into Phase 3 summary. Focus on the confidence of the valuation range and the margin of safety.

**When `apex-analysis-dcf:dcf` plugin is available but no output exists:**

Invoke the DCF plugin for this ticker first, then read the output.

## Earnings Enrichment

Earnings trajectory enriches Phase 3 regardless of whether DCF delegation was used. Forward earnings momentum directly affects what you should pay — revision direction, surprise patterns, and analyst consensus inform whether current multiples understate or overstate value.

### Earnings Delegation Path

**When `.analysis/TICKER/earnings/earnings-profile.json` exists:**

Read the earnings profile and extract:
- Composite score (0-10) and verdict (Strong Momentum / Positive / Neutral / Deteriorating / Red Flag)
- Revision momentum: net revision ratio, breadth, acceleration, EPS/revenue alignment
- Surprise history: EPS and revenue beat rates, average surprise %, quarters analyzed, surprise trend (improving/stable/narrowing/worsening), sandbagging score
- Earnings call credibility: score (if available — this is LLM-native, may be null)
- Analyst consensus: buy/hold/sell distribution, price target premium/discount to current, PT dispersion, upgrade ratio, analyst count

Wire into Phase 3 assessment:
- **Score ≥ 7 (Positive/Strong Momentum):** Earnings revisions trending up — forward multiples may understate value. Positive momentum supports a more constructive valuation range
- **Score 4-6.99 (Neutral):** No strong directional signal — rely more heavily on historical multiples and DCF
- **Score < 4 (Deteriorating/Red Flag):** Earnings revisions trending down — current multiples may overstate value. Require wider margin of safety
- **High sandbagging score (>7):** Company consistently under-promises — actual earnings power likely higher than estimates suggest
- **Revision acceleration positive:** Momentum building — consensus hasn't fully adjusted yet

**When `apex-analysis-earnings:earnings` plugin is available but no output exists:**

Invoke the earnings plugin for this ticker first, then read the output.

### Earnings Self-Contained Path

**When no earnings plugin or output available:**

Use raw FMP data to assess earnings trajectory:
- Pull analyst estimates (current year + next year, EPS and revenue)
- Pull earnings history (8+ quarters of actual vs estimate)
- Calculate beat/miss rate and average surprise magnitude
- Check estimate revision direction over last 90 days (up/down/flat)
- Pull analyst rating distribution (buy/hold/sell counts)
- Note price target consensus vs current price
- This provides a simplified version without the LLM-native credibility component

## Self-Contained Path

**When no DCF plugin or output available:**

Use raw FMP data from `.data/financial/fmp/TICKER/` to value:

### 1. Owner Earnings (Buffett's Concept)
Buffett's preferred measure of true earnings power:
- Start with net income
- Add back: depreciation, amortization
- Subtract: maintenance capex (estimate as % of total capex — typically 40-60%)
- Subtract: working capital increases
- This approximates what an owner could extract annually

### 2. Earnings Yield vs Treasury Rate
- Calculate earnings yield: inverse of P/E (E/P)
- Compare to 10-year treasury rate
- The spread is your equity risk premium for this specific stock
- **Red flag:** Earnings yield below or near treasury rate = not being compensated for equity risk

### 3. Multiple-Based Valuation Range
Build a simple valuation range using:

**P/E approach:**
- Current P/E vs 5-year average P/E
- Current P/E vs peer average P/E
- Apply normalized earnings to conservative/base/optimistic multiples
- Range: [conservative, base, optimistic]

**EV/EBITDA approach:**
- Current EV/EBITDA vs 5-year average
- Current EV/EBITDA vs peer average
- Apply current EBITDA to conservative/base/optimistic multiples
- Subtract net debt, add cash → equity value per share
- Range: [conservative, base, optimistic]

### 4. Triangulation
- Cross-check P/E range with EV/EBITDA range
- Where do they converge? That's your best estimate of fair value
- The spread of the range reflects your confidence (wider = less certain)

### 5. Margin of Safety Calculation
- Current price vs midpoint of valuation range
- Margin of safety = (fair value - current price) / fair value
- Minimum threshold: 20% for good businesses, 30%+ for uncertain ones

## Investor Principle Hooks

### Graham's Margin of Safety (Required)
> What am I paying vs what it's worth? What is the margin of safety?

State the specific percentage. If margin of safety is below 20%, this should factor heavily into the verdict. Graham's insight: you make money on the purchase price, not the sale price.

If earnings data is available: factor trajectory into the margin of safety assessment. Upward revisions mean forward earnings power is growing — the margin of safety on a forward basis may be larger than trailing multiples suggest. Downward revisions mean the opposite — don't anchor to stale earnings estimates.

### Marks' Second-Level Thinking (Required)
> What is the market pricing in? What am I seeing that the consensus doesn't?

First-level thinking: "This is a great company, buy it." Second-level thinking: "This is a great company but everyone knows it, so it's priced for perfection — any disappointment will hurt." State your specific disagreement with market consensus.

## Phase 3 Output

Produce a summary with:
- **Source:** [plugin output / self-contained]
- **Earnings source:** [earnings plugin / self-contained / not available]
- Intrinsic value range: [low, mid, high]
- Current price and margin of safety percentage
- Valuation method(s) used
- Key assumptions driving the valuation
- Earnings yield vs treasury rate spread
- Earnings trajectory (revisions up / flat / down / not assessed) — from earnings enrichment
- Graham's margin of safety answer
- Marks' second-level thinking answer
- **Phase 3 Score:** Strong / Adequate / Weak / Disqualifying

**Disqualifying conditions:**
- Negative margin of safety (trading above intrinsic value) with no clear catalyst for re-rating
- Earnings yield below treasury rate with no growth to compensate
- Valuation dependent on assumptions you cannot defend
