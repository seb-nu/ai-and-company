---
name: mbb-slide-design
description: >
  Load this skill when the user needs to design, format, or visually calibrate
  consulting slides. Covers action titles, the 1-1-1 rule, slide type selection,
  chart selection, layout patterns, and visual QA against MBB example slides.
  Trigger phrases: "action titles", "slide types", "chart selection", "slide design",
  "visual calibration", "slide patterns", "1-1-1 rule", "consulting layout",
  "improve my slides", "format slides", "slide review", "executive presentation",
  or any request about what a slide should look like visually.
  Also trigger when the user invokes /improve-slide, /slide-review, or /build-deck commands.
version: 0.1.0
---

# MBB Slide Design Standards

This skill encodes the visual and structural slide-design principles used at McKinsey, BCG, and Bain.
Apply these standards whenever creating, reviewing, or improving the layout and content of individual slides.

For argument structure and logical frameworks, see the `mbb-thinking` skill.
For company branding and template configuration, see the `mbb-template` skill.

---

## Visual Reference: Example Slides

Eight reference slides are bundled in `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/`. Read these images before building or reviewing slides to calibrate visual quality against the MBB standard:

| File | Pattern |
|---|---|
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-01.jpg` | Title slide — dark/charcoal background, burnt-orange accent, governing-thought headline |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-02.jpg` | Executive summary — 3 dark-header boxes, pyramid on one slide |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-03.jpg` | 3-pillar framework — numbered cards, blue top accent, bullets |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-04.jpg` | 2×2 matrix — 4 colored quadrants, entity dots, axis labels |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-05.jpg` | Bar chart — insight action title, callout box, source footnote |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-06.jpg` | Process roadmap — 4 phases, active phase highlighted dark/black |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-07.jpg` | Waterfall bridge — EBITDA gap, floating green bars, gridlines |
| `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/slide-08.jpg` | Recommendation — dark/black box, burnt-orange accent, Why/How/Outcome columns |

**When to read these images — read only what you need, one at a time:**
- When building or rendering a specific slide type, read only that pattern's image
- During visual QA of a rendered `.pptx`, read the relevant image(s) to compare output against the standard
- Do NOT read all 8 images at once — load only the ones relevant to the slide type you are currently working on

For layout rules and ASCII mockups of each pattern, see `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/slide-patterns.md`.
Read `slide-patterns.md` in preference to images during content drafting — it is lighter and sufficient for structure decisions. Only reach for the images when you need visual calibration (e.g. during pptx QA).

---

## Slide Anatomy

### Action Titles

Every slide title must express an **insight**, not a topic.

| Weak (descriptive) | Strong (action title) |
|---|---|
| "Revenue overview" | "Revenue grew 8% but costs outpaced at 20%, squeezing margin to 5%" |
| "Market analysis" | "Market is consolidating — top 3 players now control 70% of share" |
| "Recommendations" | "3 actions can recover $40M by Q3 without headcount cuts" |

The test: does the title answer *So what?* If not, rewrite it.

**Number conventions:**
- Action titles almost always include a number (%, $, count, ratio, timeframe) — a title with no number is often a sign it is still descriptive
- Always use numerals, not words: write **5**, **20%**, **$40M** — not *five*, *twenty percent*, *forty million dollars*

### The 1-1-1 Rule

Each slide should have:
- **1 message** — expressed in the action title
- **1 visual** — one chart, framework, or diagram (if data is needed)
- **1 takeaway** — the single thing the audience remembers

### Slide Types

| Type | When to use |
|---|---|
| Title slide | Cover; includes client name, date, confidentiality |
| Agenda / Roadmap | Show structure; revisit between major sections |
| Executive Summary | Full answer on one slide (pyramid top) |
| Recommendation slide | State the answer and implications |
| Process / Timeline | Show sequencing or phased delivery |
| Data / Chart slide | One chart, one insight |
| Backup / Appendix | Supporting detail not needed in main flow |

---

## Chart Selection

Match the chart type to the message being communicated:

| Message type | Chart type |
|---|---|
| Trend over time | Line chart |
| Part-to-whole | Stacked bar |
| Comparison (few items) | Bar or column chart |
| Correlation | Scatter plot |
| Ranking | Horizontal bar chart |
| Variance / bridge | Waterfall chart |
| Process / flow | Swimlane or flowchart |
| Geographic distribution | Map |

**Chart title = the insight, not the axis label.**
Write "Customer acquisition cost doubled since 2022" — not "CAC 2019–2024."
