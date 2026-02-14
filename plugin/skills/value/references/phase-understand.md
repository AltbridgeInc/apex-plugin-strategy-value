# Phase 1: Understanding the Business

Determine whether this is a business you can understand, whether it has a durable competitive advantage, and whether it creates value for shareholders.

## Delegation Path

**When `.analysis/TICKER/quality/` exists:**

Read the quality assessment outputs and extract:
- Moat type and durability assessment
- ROIC trend (5Y)
- Quality score and key quality signals
- Industry structure and competitive position
- Management assessment

Synthesize into Phase 1 summary. Don't repeat the quality report — extract what matters for the investment decision.

**When `apex-analysis-quality:quality` plugin is available but no output exists:**

Invoke the quality plugin for this ticker first, then read the output.

## Self-Contained Path

**When no quality plugin or output available:**

Use raw FMP data from `.data/financial/fmp/TICKER/` to assess:

### 1. Business Model
- Read company profile — what do they do, how do they make money?
- Revenue breakdown by segment/geography if available
- Apply **Li Lu's simplicity test**: can you explain this in one sentence?

### 2. Moat Identification
Identify which moat type(s) apply:
- **Brand/pricing power** — gross margins stable or expanding, premium to peers
- **Switching costs** — high retention, recurring revenue, integration depth
- **Network effects** — value increases with users, marketplace dynamics
- **Cost advantage** — scale economies, proprietary process, geographic
- **Regulatory/license** — barriers to entry from regulation

### 3. ROIC Check (Quick)
From financial statements (5Y):
- Calculate ROIC = NOPAT / Invested Capital
- Is ROIC consistently above WACC (use 10% as rough proxy)?
- Is ROIC trending up, stable, or declining?
- Compare to industry average if peer data available

### 4. Circle of Competence
- Is this industry one you can reason about?
- Are the value drivers understandable (not dependent on unpredictable technology bets)?
- Can you identify when the thesis would be wrong?

## Investor Principle Hooks

### Li Lu's Simplicity Test (Required)
> Describe this business in one sentence.

If you can't, that's a red flag — either the business is too complex or you don't understand it well enough. Both are reasons for caution.

### Buffett's 10-Year Moat Question (Required)
> Will this competitive advantage be stronger in 10 years?

Consider: Is the moat widening (network effects compounding, brand strengthening)? Or narrowing (technology disruption, regulation changing, competitors catching up)?

## Phase 1 Output

Produce a summary with:
- **Source:** [plugin output / self-contained]
- Business description (1-2 sentences)
- Moat type and durability assessment
- ROIC trajectory signal (creating value / destroying value / unclear)
- Circle of competence: yes / partially / no
- Li Lu's simplicity test answer
- Buffett's 10-year moat answer
- **Phase 1 Score:** Strong / Adequate / Weak / Disqualifying

**Disqualifying conditions:**
- Cannot explain the business simply (fails Li Lu test)
- ROIC consistently below cost of capital with no turnaround path
- Moat clearly narrowing with no offsetting factors
