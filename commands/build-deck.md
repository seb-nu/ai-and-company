---
description: Build an MBB-style deck from a brief or raw notes
allowed-tools: Read, Write, Bash
argument-hint: [topic or file path]
---

Check for user config and load it if present:
```bash
cat ~/.claude/ai-and-company/USER_CONFIG.md 2>/dev/null || echo "NO_USER_CONFIG"
```
If the output is `NO_USER_CONFIG`, use MBB defaults (English, formal tone, AI&Company palette) and note to the user that they can run `/setup` to configure their company template and preferences.

Load the MBB consulting methodology for this session:
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-thinking/SKILL.md
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/SKILL.md
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-template/SKILL.md
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-qa/SKILL.md

Apply all settings from USER_CONFIG.md throughout this command:
- Use the specified output language for all slide content
- Apply the company's theme colors to any chart or visual descriptions
- Use the company's terminology preferences and tone
- Use the confidentiality label and numbering format in footers
- Reference the company name where appropriate

Build a consultant-quality PowerPoint deck on the following topic or brief: $ARGUMENTS

Follow these steps in order:

## Step 1 — Understand the Brief

If a file path was provided, read it using the Read tool. If only a topic was given, ask the user these three questions before proceeding:
1. Who is the audience and what decision will they make after seeing this deck?
2. What is your hypothesis or recommendation (even a rough one)?
3. What data or information is available to support it?

Do not start structuring until you have answers to all three.

## Step 2 — Draft the Storyline

Before writing any slide body content, create the full action-title outline internally:
- Apply SCR framing: Situation → Complication → Resolution
- Write one action title per slide (complete sentences expressing insights, not topics)
- Aim for 8–20 slides depending on brief complexity
- Include: Title, Executive Summary, Agenda, Situation (2–3 slides), Complication (2–3 slides), Recommendation (3–5 slides), Next Steps

Do not present the storyline to the user yet. Proceed directly to Step 3.

## Step 3 — Check Horizontal Logic

Read the action titles sequentially as if they were sentences in a paragraph. Verify:
- The story flows logically from slide to slide
- The Executive Summary action title = the governing thought (the full recommendation)
- All Situation/Complication slides set up the Resolution slides
- MECE check: are the major sections mutually exclusive and collectively exhaustive?

Fix any gaps. Then present the finalised storyline to the user as a numbered list of action titles only.
**Wait for explicit confirmation before proceeding to Step 4.**

## Step 4 — Build Slide Content

**Important: do not load the pptx skill, read example images, or write any code in this step. Text content only.**

For each slide in the confirmed storyline:
- **Action title**: Already written — do not change without noting it
- **Body content**: 3–5 supporting points OR describe one chart/visual with its title and key data
- **Chart title** (if applicable): Express the insight, not the axis label
- Apply the 1-1-1 rule: one message, one visual, one takeaway

Write all slides in this format:
```
## Slide [N]: [Action Title]
Body: [bullet points or chart description]
Visual: [chart type and data, if applicable]
```

Before presenting to the user, run the full **MBB QA Checklist** from `mbb-qa` on each slide — all seven sections (Required Elements, Layout & Visual Integrity, Typography & Formatting, Text & Language, Numerical Consistency, Chart Standards, Consulting Quality). Fix any failures silently. Collect items that cannot be resolved (e.g., missing source, unverified numbers) into a **"Flags for your attention"** section appended after the last slide.

Present the full slide-by-slide content to the user, followed by the Flags section (if any).
Flag any slides where data is assumed or missing, and suggest what analyses would strengthen the argument.
**Wait for explicit confirmation before proceeding to Step 5.**

## Step 5 — Deliver

Now invoke the `anthropic-skills:pptx` skill to build the `.pptx` file from the confirmed slide content, unless the user has explicitly asked for a different format.
Instruct the pptx skill to use **python-pptx** for file generation:

```bash
ls ~/.claude/ai-and-company/template.pptx 2>/dev/null
```

- **If the user's template exists** → `Presentation("~/.claude/ai-and-company/template.pptx")`. Load `@~/.claude/ai-and-company/TEMPLATE.md` for the layout map and use the Purpose column to select the correct `slide_layouts[index]`.
- **If no user template** → `Presentation("${CLAUDE_PLUGIN_ROOT}/skills/mbb-template/assets/AI&Company Template.pptx")`. Use the hardcoded layout indexes from the mbb-template skill.
