---
description: Exploratory data analysis using MBB consulting lenses — outputs action-title insights with structured tables in chat
allowed-tools: Read
argument-hint: [file path, pasted data, or topic — optionally followed by areas of particular interest]
---

Check for user config and load it if present:
```bash
cat ~/.claude/ai-and-company/USER_CONFIG.md 2>/dev/null || echo "NO_USER_CONFIG"
```
If the output is `NO_USER_CONFIG`, use MBB defaults (English, formal executive tone).

Load the MBB consulting methodology for this session:
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-thinking/SKILL.md

Apply all settings from the loaded user config throughout this command:
- Use the specified output language for all insight titles, tables, and commentary
- Apply the company's terminology preferences (e.g. "clients" not "customers" if specified)
- Match the company's tone throughout the analysis

Run a consultant-quality exploratory analysis on the following: $ARGUMENTS

If a file path is provided, read it first using the Read tool. If data is pasted inline, use it directly. If the user has flagged specific areas of interest, treat those as priority lenses — analyse them first and in more depth — but do not stop there; run the full scan. If no data is provided at all, ask for it before proceeding. Never fabricate numbers.

All output goes to chat. Do not write any files unless the user explicitly asks.

---

## Step 1 — Ingest & Frame

Parse the data and establish the analytical context before writing anything to the user.

Identify:
- What is being measured and what a single row represents
- The time range and granularity (annual, quarterly, monthly, daily)
- The key dimensions available (segments, clients, products, geographies, etc.)
- Any data quality issues: missing periods, gaps, inconsistent units, ambiguous column names — flag these explicitly; do not silently fill them

Then form:
- **Governing question**: the one question a decision-maker would most want this data to answer. Write it as a complete sentence.
- **Initial hypothesis**: a directional one-sentence answer, committed to before the analysis. This will be tested, not assumed.

Present a brief dataset profile to the user, then proceed immediately to Step 2 without waiting for confirmation:

```
DATASET PROFILE
- Rows / observations: [N]
- Key columns: [list]
- Time range: [earliest] to [latest] ([N periods], [granularity])
- Key dimensions: [segments, entities, etc.]
- Data quality: [gaps or issues — "None detected" if clean]
- Units: [currency, volume, %, etc.]

GOVERNING QUESTION
[One sentence]

INITIAL HYPOTHESIS
[One sentence — directional answer before analysis]
```

---

## Step 2 — Analytical Scan

Run an exhaustive scan. The lenses below are a guide — apply all that fit, skip those that do not, and add others if the data warrants. Derive as many findings as the data supports; there is no cap.

Work through all lenses internally first, then format the findings in Step 3.

### Scale & Baseline
What is the absolute level of the key metric today (or in the most recent period)? How does it compare to a reference: prior year, plan, or a natural benchmark? Is the scale large or small relative to the question being asked?

### Trend
How has the key metric moved over time? Calculate YoY growth for each period and CAGR across the full range. Is growth accelerating or decelerating? Is there a visible inflection point or reversal? Which periods were outliers?

- YoY growth: `(Current − Prior) / Prior × 100`
- CAGR: `(End Value / Start Value)^(1 / Years) − 1`

### Mix
How is the total composed? Which segments, clients, products, or geographies drive the result, and how has their share shifted over time? Apply the MECE check: do the parts add to the whole?

### Comparison
How does the subject compare against a benchmark, plan, prior period, or peer? Which entities outperform or underperform, and by how much?

### Driver Decomposition
What explains the pattern? Decompose the key metric into its additive or multiplicative components (e.g. Revenue = Volume × Price × Mix; Growth = Organic + Inorganic; Profit = Revenue − Fixed Costs − Variable Costs). Attribute the change between two periods to specific drivers. Quantify each driver's contribution.

### Concentration & Outliers
Apply the Pareto principle: which ~20% of items drive ~80% of the result? Are there outliers — values 2× or more above or below the group average? Is there a meaningful long tail? Rank all entities and identify the extremes.

---

## Step 3 — Synthesize & Deliver Insights

For each finding, deliver an insight card in this exact format:

---
**Insight [N]: [Action title]**

[Interpretation]

[Table]

**Implication**: [So what]

---

**Action title rules:**
- Complete sentence — not a label or topic header
- Must contain at least one specific number (%, $, ratio, count, or timeframe)
- Must answer "So what?" — the reader should understand the finding from the title alone
- Bad: "Revenue by client 2023–2024" / Good: "Client A alone drives 41% of H1 revenue, creating significant concentration risk"

**Interpretation:** 1–2 sentences explaining what is driving the finding and why it matters. Do not restate what the table shows — the table is the evidence; the interpretation is the argument.

**Table:** The table is the full underlying data, not a summary of the headline. If the insight says "Client A = ~40% of revenue," the table shows all clients broken down by period with totals — so the reader sees the complete picture, not just the cited number. Choose the structure that best fits the finding.

**Implication:** One sentence stating what this means for the business or the decision at hand.

Order insights from most to least impactful relative to the governing question. If the user flagged specific areas, lead with those.

---

## Table Guidance

Tables are evidence, not captions. Show the full data behind the insight — more context convinces; less context just asserts.

### Time-based data: time goes in columns

Management reports and consulting exhibits put time periods in columns so readers can scan across periods and compare metrics down the rows. Apply this consistently.

**Example — revenue by client over time:**

| Client | Jan | Feb | Mar | Apr | May | Jun | H1 Total | Share |
|--------|-----|-----|-----|-----|-----|-----|----------|-------|
| Client A | 28 | 31 | 27 | 33 | 30 | 35 | **184** | **41%** |
| Client B | 18 | 19 | 20 | 21 | 19 | 22 | 119 | 26% |
| Client C | 14 | 12 | 15 | 13 | 16 | 14 | 84 | 19% |
| Others | 8 | 9 | 7 | 9 | 10 | 8 | 51 | 11% |
| **Total** | **68** | **71** | **69** | **76** | **75** | **79** | **438** | **100%** |

This table proves "Client A = ~40% of revenue" with full context across all clients and all months.

**Example — multi-year trend with CAGR:**

| Metric | 2020 | 2021 | 2022 | 2023 | 2024 | CAGR [2020–2024] |
|--------|------|------|------|------|------|-----------------|
| Revenue ($M) | 210 | 231 | 262 | 278 | **312** | **+10.4%** |
| Gross Margin (%) | 54% | 55% | 53% | 51% | **49%** | — |
| YoY Growth | — | +10.0% | +13.4% | +6.1% | **+12.2%** | — |

CAGR goes in the final column, calculated only for metrics where it is meaningful. Label the column with the full year range: `CAGR [YYYY–YYYY]`.

### Bridge / Waterfall

Use for attributing change between two values to specific drivers. Positive drivers first (largest first), then negative, then the net change. Verify the column sums exactly.

| Driver | Δ Revenue ($M) | Notes |
|--------|---------------|-------|
| Volume growth | +44 | 8 new enterprise accounts |
| Price realization | +12 | Avg contract value +4% |
| Mix shift to enterprise | +9 | Enterprise share +8pp |
| SMB churn | −31 | Elevated H1 losses |
| **Net change FY23→FY24** | **+34** | **+12.2%** |

### Ranking with context

Include share and delta so the ranking tells a richer story than just the order.

| Rank | Client | Revenue ($M) | Share (%) | Δ YoY ($M) |
|------|--------|-------------|-----------|-----------|
| 1 | Acme Corp | **48** | **15%** | +12 |
| 2 | GlobalTech | 39 | 13% | +6 |
| 3 | Meridian | 27 | 9% | −3 |
| … | … | … | … | … |
| **Total** | — | **312** | **100%** | **+34** |

### Column conventions

Apply these to every table without exception:

- **Units in the header, not the cells.** Write `Revenue ($M)` in the header; write `312` in the cell — not `$312M`.
- **Delta columns always show the sign.** Write `+34` and `−12`, never `34` or `12`.
- **Percentage points use `pp`.** The difference between two percentages is always expressed in pp, not %: `+8pp`, `−3pp`.
- **CAGR label includes the year range.** `CAGR [2020–2024]` — never just `CAGR`.
- **Total / subtotal rows are always bold and always last.**
- **Bold the key value(s) within a row** to guide the reader's eye to the most important number.
- **No merged cells.** Use standard markdown table syntax only.
- **Only include columns that add information.** There is no column limit, but cut anything redundant.

---

## Step 4 — Gaps & Next Steps

After all insights are presented, close with:

**What we cannot yet answer**
List 2–4 specific questions the data does not resolve. For each, state what additional data would be needed to answer it.

**Recommended next analyses**
List 2–3 targeted follow-on analyses that would deepen the most important insight or test the hypothesis. Frame each as an action, not a topic.

**Hypothesis verdict**
Return to the hypothesis from Step 1. State in one sentence whether the analysis supports, refutes, or is inconclusive on it — and briefly why.

---

Then add:

> If these insights look right, I can build a structured deck from them using `/build-deck` — each insight becomes a chart slide, and the governing question becomes the executive summary. Let me know if you'd like to proceed, or if any insight needs to be revised first.
