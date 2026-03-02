---
name: mbb-thinking
description: >
  Load this skill when the user needs to structure a consulting argument, apply
  the Pyramid Principle, use MECE structuring, build an SCR narrative, design
  a deck storyline, or plan the logical structure of a presentation. Trigger
  phrases: "pyramid principle", "MECE", "governing thought", "SCR",
  "structure my argument", "consulting framework", "horizontal logic",
  "vertical logic", "deck structure", "storyline", "executive summary structure",
  "consultant slides", "McKinsey style", "BCG deck", "Bain presentation",
  "build a deck", "slide storyline", "consulting deck",
  "exploratory data analysis", "analyze this data", "what do the numbers show",
  "insight from data", "data scan", "what's driving", "decompose this metric",
  or any request to organise thinking in consultant style.
  Also trigger when the user invokes /structure-argument, /build-deck, or /explore commands.
version: 0.1.0
---

# MBB Thinking Frameworks

This skill encodes the argument-structure and analytical thinking principles used at McKinsey, BCG, and Bain.
Apply these standards whenever building a deck storyline, structuring an argument, or running a consulting-style analysis.

When creating actual `.pptx` files, also invoke the `anthropic-skills:pptx` skill for file handling, `mbb-slide-design` for visual standards, and `mbb-template` for company configuration and template.
Always instruct the pptx skill to use **python-pptx** (not pptxgenjs) for all file operations.

---

## Core Principles

### 1. The Pyramid Principle (Barbara Minto)

Every deck and every slide communicates top-down:

- **Start with the answer** — the governing thought / key message comes first
- **Support with arguments** — 3 is the ideal number; 5 is the maximum
- **Support arguments with data** — facts and evidence, not more conclusions

Never bury the punchline. The audience should know your recommendation by slide 2.
See `${CLAUDE_PLUGIN_ROOT}/skills/mbb-thinking/references/pyramid-principle.md` for detailed guidance and worked examples.

### 2. MECE (Mutually Exclusive, Collectively Exhaustive)

Every time you group ideas, check:

- **Mutually Exclusive**: No overlap between categories
- **Collectively Exhaustive**: No gaps — together they cover everything

Apply MECE to: agenda sections, issue trees, option sets, sub-bullets on a slide.
See `${CLAUDE_PLUGIN_ROOT}/skills/mbb-thinking/references/mece-framework.md` for MECE patterns and common failure modes.

### 3. SCR Storyline (Situation–Complication–Resolution)

Use SCR to shape and pressure-test the narrative logic internally — it is a thinking framework for the consultant, not a labeling system for slides. The audience should never see slides titled "Situation", "Complication", or "Resolution".

- **Situation**: Context the audience already accepts ("Our costs have risen 20% YoY")
- **Complication**: The tension or problem that disrupts the situation ("Revenue grew only 8%")
- **Resolution**: Your recommendation or answer ("We must cut $40M in OpEx by Q3")

The SCR logic shapes which content goes where and in what order; the slides themselves carry insight-driven action titles.

### 4. Horizontal Logic (Slide-to-Slide)

Reading only the action titles of every slide should tell the full story.
Test: strip all body content and read titles in sequence — they must form a coherent argument.

### 5. Vertical Logic (Within a Slide)

The slide title must be supported by the body content.
If the title says "Costs are growing faster than revenue," every element on that slide must prove it.

---

## Deck Structure Templates

**Standard recommendation deck (15–25 slides):**
1. Title slide
2. Executive Summary (the full answer in one slide)
3. Agenda
4. Context-setting slides (2–3) — SCR "Situation" in your thinking
5. Problem/tension slides (2–3) — SCR "Complication" in your thinking
6. Recommendation slides (3–5) — SCR "Resolution" in your thinking
7. Implementation roadmap
8. Appendix / Backup

**Short update / steering deck (6–10 slides):**
1. Title
2. Executive Summary
3. Key findings (2–3 slides)
4. Recommendation
5. Next steps
