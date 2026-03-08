---
description: Rewrite slides to MBB consulting quality
allowed-tools: Read, Write
argument-hint: [slide content, file path, or paste slide text]
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
- Use the specified output language for all rewritten slide content
- Apply the company's terminology preferences (e.g. "clients" not "customers")
- Match the company's tone setting
- When describing chart colors or callout styles, reference the company's defined palette

Improve the following slide(s) to MBB consulting standard: $ARGUMENTS

If a `.pptx` file path was provided, invoke `anthropic-skills:pptx` to read the file first, then apply the improvements below.

Apply improvements in this order for each slide:

## 1. Rewrite the Action Title

The current title likely describes the topic. Rewrite it to express the insight — the "so what."

Test: Does the new title answer a meaningful question by itself?

| Before | After |
|---|---|
| "Revenue overview" | "Revenue grew 8% but costs outpaced at 20%, squeezing margin to 5%" |
| "Market trends" | "Three structural shifts are permanently compressing industry margins" |
| "Team capabilities" | "Current team lacks two critical capabilities needed for the new strategy" |

## 2. Check Vertical Logic

Does every element in the slide body directly support the action title?
- Remove anything that does not prove the title
- Add what is needed to make the case bulletproof
- If the body proves a different point than the title, either change the title or change the body — not both

## 3. Simplify and Tighten

- Maximum 5 bullet points per slide — if more, group them
- Each bullet = one complete thought expressing an insight (not a label)
- Remove any bullet that restates what the chart already shows
- Cut filler words: "It is important to note that…", "In order to…", "Various…"

## 4. Improve the Chart (if applicable)

- Chart title must express the insight ("Acquisition cost doubled" not "CAC over time")
- Chart type must match the message — flag if it doesn't (see skill for chart selection guide)
- Remove chart junk: 3D effects, excess gridlines, redundant legends, more than 4 colors
- Add annotation or callout pointing to the key data point

## 5. MECE Check

If the slide has grouped sub-points or categories, verify:
- No two bullets say the same thing (mutually exclusive)
- Together, the bullets cover the full argument (collectively exhaustive)

## 6. Flag Data Issues

Note any numbers that seem inconsistent with context, or where the data behind a claim is unclear.

## 7. Run Per-Slide QA Check

After applying all improvements above, run the full **MBB QA Checklist** from `mbb-qa` against the rewritten slide — all seven sections (Required Elements, Layout & Visual Integrity, Typography & Formatting, Text & Language, Numerical Consistency, Chart Standards, Consulting Quality).

Fix any remaining failures silently before presenting output to the user. If a check cannot be resolved from available information (e.g., source is unknown, slide number is not specified), add it to the change log as a flagged item requiring user input — do not invent a source or number.

---

## Output Format

For each slide, present:
1. **Rewritten slide** (title + body, ready to paste)
2. **Change log** — a brief explanation of each change and the reason behind it

If multiple slides were provided, process them sequentially and maintain consistent terminology across slides.

Once the rewritten slides have been presented and confirmed, invoke the `anthropic-skills:pptx` skill to write the improved content back into a `.pptx` file using **python-pptx**, unless the user has explicitly asked for a different format.

Use the same template resolution as all other commands:
- User template: `~/.claude/ai-and-company/template.pptx` (load `@~/.claude/ai-and-company/TEMPLATE.md` for layout indexes)
- Fallback: `${CLAUDE_PLUGIN_ROOT}/skills/mbb-template/assets/AI&Company Template.pptx`
