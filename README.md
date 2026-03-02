# AI&Company

A consulting-grade PowerPoint toolkit for Claude, encoding the slide-making standards used at McKinsey, BCG, and Bain. Covers the Pyramid Principle, MECE structuring, action titles, storyline design, and deck quality review.

Works alongside the `anthropic-skills:pptx` skill to handle actual `.pptx` file creation and editing.

---

## Commands

### `/build-deck [topic or file]`
Build a full MBB-style deck from scratch. Claude drafts the storyline (action titles only) first and waits for your sign-off before filling in slide content. Optionally produces a `.pptx` file.

### `/improve-slide [slide content or file]`
Rewrites slides to consulting standard: action titles, vertical logic, MECE bullets, chart improvements. Returns the rewritten slide plus a change log explaining each edit.

### `/structure-argument [topic or notes]`
Structures a consulting argument using the Pyramid Principle. Produces a governing thought, SCR narrative, and full pyramid (Level 1 → 2 → 3) with proposed action titles for each slide.

### `/slide-review [file or slide content]`
Audits a deck for: numerical inconsistencies across slides, grammar and language errors, and consulting quality issues (descriptive titles, multi-message slides, etc.). Ends with 3–5 targeted follow-up questions.

---

## Skill

The `mbb-consulting` skill is loaded automatically when you use any of the commands above, or when you ask Claude about consulting slides, MBB style, the Pyramid Principle, MECE, or executive presentations.

The skill includes detailed reference material on:
- **Pyramid Principle** — governing thought, inductive/deductive grouping, SCQA, executive summary structure
- **MECE Framework** — standard MECE patterns, quick tests, common failure modes
- **Slide Design Standards** — typography, color, chart formatting, layout, footer conventions

---

## Setup

No environment variables or external services required. Works out of the box.

For `.pptx` file creation, ensure the `anthropic-skills:pptx` skill is also installed.

### Company Configuration (`USER_CONFIG.md`)

After installing, open `USER_CONFIG.md` inside the plugin folder and fill in your company's settings once. Every command loads this file automatically on every run.

Fields you can configure:

- **Company name & industry** — helps Claude tailor terminology and examples
- **PPT theme** — paste your theme colors (hex codes) or describe your visual style
- **Chart & callout colors** — primary, secondary, positive/negative indicator, and callout box colors
- **Output language** — e.g. English (UK), Spanish, Portuguese (BR)
- **Terminology preferences** — words you always use or avoid (e.g. "clients" not "customers")
- **Tone** — formal executive, direct, conservative, etc.
- **Slide standards** — confidentiality label, numbering format, dimensions

Leave any field blank to use MBB defaults.

---

## Typical Workflows

**Building a deck from a brief:**
1. `/structure-argument [your topic or notes]` — get the pyramid and storyline
2. Review and confirm the structure
3. `/build-deck [topic]` — build the full deck using the confirmed structure

**Cleaning up an existing deck:**
1. `/slide-review [your file]` — find all errors and inconsistencies
2. `/improve-slide [specific slides]` — rewrite individual slides to consulting standard

**Quick slide fix:**
- Paste slide content directly into `/improve-slide` without a file
