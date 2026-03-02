---
name: mbb-qa
description: >
  Load this skill when conducting a quality review of consulting slides or a
  full deck. Contains the per-slide QA checklist Claude must run on every slide
  it processes, builds, or improves. Trigger phrases: "review my deck",
  "quality check", "audit slides", "QA the deck", "find errors", "check my
  slides", "common mistakes", or any request to evaluate or audit existing
  presentation content. Also trigger when the user invokes /slide-review,
  /improve-slide, or /build-deck commands.
version: 0.2.0
---

# MBB Slide QA

This skill contains the per-slide quality assurance checklist for consulting slides.
Run the full checklist on every slide you process, build, or review — not just as background knowledge, but as an active pass before presenting output to the user.

For consulting thinking standards, see `mbb-thinking`. For visual design standards, see `mbb-slide-design`.

---

## Per-Slide QA Checklist

Run all seven sections on each slide. Flag every failed item with the slide number and a one-line description of the issue. Correct failures silently where you have enough information; flag items that require user input.

---

### A. Required Elements

- [ ] **Action title present** — not blank, not a placeholder like "Slide title"
- [ ] **Title is an action title** — complete sentence expressing an insight with ≥1 specific number; not a topic label ("Revenue overview" fails; "Revenue grew 8% YoY while costs rose 20%" passes)
- [ ] **Subtitle present** where the slide type calls for one (agenda sections, section dividers, two-part recommendation slides)
- [ ] **Source/footnote** on every slide that contains data, a chart, or a cited claim
- [ ] **Slide number** in the footer
- [ ] **Date** in the footer or title slide
- [ ] **Confidentiality label** per USER_CONFIG (e.g., "Confidential", "For internal use only")

---

### B. Layout & Visual Integrity

- [ ] All text is within safe margins — nothing touching slide edges or visually cut off
- [ ] No text boxes overlapping each other
- [ ] No text overlapping charts, images, or other visual elements
- [ ] All elements are visibly aligned to a consistent grid (no skewed, floating, or randomly placed boxes)
- [ ] Left and right margins are consistent across all text columns on the slide
- [ ] Nothing bleeds beyond the slide boundary

---

### C. Typography & Formatting

- [ ] Title font size ≥ 24pt
- [ ] Body text font size ≥ 16pt
- [ ] Source/footnote font size ≥ 10pt
- [ ] Capitalization style is consistent with the rest of the deck (title case or sentence case — never mixed on the same slide)
- [ ] Bullet punctuation is consistent: either all bullets end with a period or none do — no mixing
- [ ] No more than 2 distinct font sizes in the body area of a single slide
- [ ] Bold is used only for emphasis, not decoration or random highlighting
- [ ] Colors match the company palette defined in USER_CONFIG

---

### D. Text & Language

- [ ] No spelling errors or typos in any text element (title, bullets, chart labels, footnotes, callouts)
- [ ] Subject-verb agreement correct throughout; no unintentional sentence fragments
- [ ] Punctuation correct: proper apostrophes, no comma splices, no double spaces
- [ ] Terminology matches USER_CONFIG preferred terms (e.g., flag "customers" if config specifies "clients")
- [ ] Tense is consistent across the slide (not mixing past and present without cause)
- [ ] No filler phrases: "It is important to note that", "Various", "In order to", "Please note that", "As mentioned above"
- [ ] Numbers ≥ 10 written as numerals (write 12, not twelve); numbers < 10 may be words or numerals — be consistent
- [ ] Every number has a unit: $M, %, bps, x, pp, #, etc.

---

### E. Numerical Consistency

- [ ] Every number on the slide has a clear unit and time period
- [ ] Derived figures are internally consistent — check the arithmetic (Revenue × Margin % = Profit; components sum to total)
- [ ] Percentages that are parts of a whole sum to 100% (flag if they don't and no rounding note is present)
- [ ] Year/period labels are unambiguous and consistent with how they appear elsewhere in the deck (FY24 vs. CY24 vs. 2024 — pick one and use it throughout)
- [ ] Rounding is consistent — if a figure appears elsewhere as "34%", it should not appear here as "33.6%" without explanation
- [ ] CAGR labels include the full year range: "CAGR '21–'24" not just "CAGR"
- [ ] Cross-slide check: if the same metric appeared on a previous slide, confirm the values match; flag any discrepancy

---

### F. Chart Standards *(skip if no chart on this slide)*

- [ ] Chart title expresses an insight, not an axis label — "Acquisition cost doubled since 2022" passes; "CAC 2019–2024" fails
- [ ] Chart type matches the message:
  - Trend over time → line chart
  - Comparison (few items) → bar or column chart
  - Part-to-whole → stacked bar or pie (pie only if ≤ 3 segments)
  - Ranking → horizontal bar chart
  - Variance / bridge → waterfall chart
  - Correlation → scatter plot
  - Geographic distribution → map
- [ ] Source footnote present on the chart (in addition to any slide-level footnote)
- [ ] No 3D effects on any chart element
- [ ] No more than 4 colors used in a single chart
- [ ] No redundant legend when data labels are already on bars, lines, or pie segments
- [ ] Key data point (the one proving the title) has an annotation or callout box pointing to it
- [ ] Y-axis starts at 0 for bar and column charts — flag if it doesn't (truncated axes mislead); an exception is allowed only if the deviation from zero is itself the story, and that must be noted
- [ ] No excess gridlines (remove horizontal gridlines if data labels are present)
- [ ] Axis labels carry units in the header, not repeated in every tick mark or cell

---

### G. Consulting Quality

- [ ] **1-1-1 rule**: the slide has exactly one message (the action title), one primary visual or evidence block, and one clear takeaway — flag any slide trying to say two things
- [ ] **Bullet count**: no more than 5 bullet points; if more exist, they must be grouped under sub-headers
- [ ] **Bullets express insights**: each bullet is a complete sentence with a "so what" — not a label or a noun phrase ("Revenue declined due to price erosion" passes; "Revenue" fails)
- [ ] **No chart echo**: no bullet repeats what the chart already shows visually — bullets must add interpretation, not restate data
- [ ] **Vertical logic**: every element (bullet, chart, callout) directly supports the action title; flag anything that is tangential or contradicts the title
- [ ] **Self-contained**: the slide's point is clear to a reader with no presenter — flag if the slide only makes sense with spoken explanation
- [ ] **MECE**: if the slide has grouped sub-points or categories, verify they are mutually exclusive (no overlap) and collectively exhaustive (no gaps); flag if a catch-all bucket exists

---

## Escalation Protocol

After running the checklist on all slides, escalate issues in three tiers:

**Must fix** — factual errors, numerical inconsistencies, missing required elements (title, source, slide number), overlapping content obscuring information
**Should fix** — grammar errors, terminology inconsistencies, bullet punctuation mixing, chart type mismatches
**Consider improving** — consulting quality flags (descriptive titles, chart echo, MECE gaps, self-contained issues)
