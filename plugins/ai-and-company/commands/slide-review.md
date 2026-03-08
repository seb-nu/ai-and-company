---
description: Audit a deck for errors, inconsistencies, and quality issues
allowed-tools: Read, Write
argument-hint: [file path or paste slide content]
---

Check for user config and load it if present:
```bash
cat ~/.claude/ai-and-company/USER_CONFIG.md 2>/dev/null || echo "NO_USER_CONFIG"
```
If the output is `NO_USER_CONFIG`, use MBB defaults (English, formal executive tone).

Load the MBB consulting methodology for this session:
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/SKILL.md
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-template/SKILL.md
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-qa/SKILL.md

Apply all settings from the loaded user config throughout this command:
- Use the specified output language for all review feedback and follow-up questions
- Flag terminology inconsistencies against the company's preferred terms (e.g. if config says use "clients" and the deck says "customers", flag it)
- When suggesting color fixes, reference the company's defined chart and callout palette
- Apply the company's tone setting when writing follow-up questions

Review the following presentation for errors and quality issues: $ARGUMENTS

If a `.pptx` file path was provided, invoke `anthropic-skills:pptx` to extract the slide content first, then conduct the review below.

---

## Per-Slide QA Pass

Before aggregating findings, run the full **MBB QA Checklist** from `mbb-qa` on every slide individually. For each slide, go through all seven sections (Required Elements, Layout & Visual Integrity, Typography & Formatting, Text & Language, Numerical Consistency, Chart Standards, Consulting Quality) and record every failed check item. This becomes the raw input for the three review dimensions below.

---

## Review Dimension 1 — Numerical Consistency

Build a reference table of every number that appears in the deck: revenues, costs, percentages, headcount, dates, growth rates, market sizes, and any other figures.

For each number, track: the value, the slide it appears on, and the context (what it refers to).

Then cross-check systematically:
- Does the same metric appear with different values on different slides?
- Do derived figures match their components? (e.g., if Slide 4 shows Revenue = $100M and Cost = $80M, does Slide 7 show a 20% margin — or something inconsistent?)
- Are percentage changes consistent with the absolute numbers shown?
- Are time periods consistent? (e.g., "2023 revenue" on Slide 3 vs. "FY23 revenue" on Slide 8 — same number?)
- Flag even minor rounding inconsistencies (e.g., "34%" on slide 3 vs. "33.6%" on slide 7)

## Review Dimension 2 — Grammar and Language

Check every text element across the deck:
- Spelling errors and typos
- Grammar errors (subject-verb agreement, tense consistency, sentence fragments)
- Punctuation errors (inconsistent use of periods at end of bullets, incorrect apostrophes)
- Inconsistent terminology (e.g., "customers" vs. "clients," "revenue" vs. "sales," "team" vs. "organization")
- Inconsistent capitalization of proper nouns, product names, or titles
- Awkward phrasing or unclear sentences
- Tense inconsistency across the deck (mixing past/present/future without cause)

## Review Dimension 3 — Consulting Quality (Bonus Observations)

While reviewing, also flag:
- Descriptive slide titles that should be action titles (titles that state a topic, not an insight)
- Slides with more than one key message
- Charts whose titles don't express an insight (axis-label titles)
- Bullets that just repeat what the chart shows
- Logical gaps in the storyline (a claim on one slide not set up by earlier slides)

---

## Output Format

Structure the review as:

### Numerical Issues
[For each issue]: **Slide X vs. Slide Y** — "[Description of inconsistency]"
Example: **Slide 4 vs. Slide 9** — "EBITDA margin shown as 18% on Slide 4 but calculated as 21% from the revenue/cost figures on Slide 9."

If no numerical issues are found, state: "No numerical inconsistencies detected."

### Language Issues
[For each issue]: **Slide X** — "[Description]" → Suggested correction
Example: **Slide 6** — "Comma splice in second bullet." → "Split into two sentences."

If no language issues are found, state: "No language issues detected."

### Quality Suggestions
[For each]: **Slide X** — "[What to improve and why]"

---

## Follow-Up Questions

After the review, ask 3–5 targeted questions to clarify ambiguities that could affect the deck's argument or accuracy. Focus on:
- Numbers that appear without a stated source or time period
- Claims that seem strong but lack supporting data on the slide
- Decisions about structure or emphasis (e.g., "Slide 12 presents two options — do you have a recommendation, or is this intentionally open?")
- Any data that seems internally consistent but potentially outdated
- Terminology choices that might be read differently by the intended audience

Frame questions as: "To finalize the review / strengthen the deck, could you clarify: [question]?"
