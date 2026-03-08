---
description: Structure a consulting argument using the Pyramid Principle
allowed-tools: Read
argument-hint: [topic, question, or raw notes / file path]
---

Check for user config and load it if present:
```bash
cat ~/.claude/ai-and-company/USER_CONFIG.md 2>/dev/null || echo "NO_USER_CONFIG"
```
If the output is `NO_USER_CONFIG`, use MBB defaults (English, formal executive tone).

Load the MBB thinking methodology for this session:
@${CLAUDE_PLUGIN_ROOT}/skills/mbb-thinking/SKILL.md

Apply all settings from the loaded user config throughout this command:
- Use the specified output language for all written content
- Apply the company's terminology preferences throughout
- Match the company's tone when writing the governing thought, SCR narrative, and slide titles

Structure a consulting argument on the following: $ARGUMENTS

If a file path was provided, read it first. Then apply the Pyramid Principle methodology in full:

## Step 1 — Identify the Governing Thought

Write one sentence that captures the single overarching answer, recommendation, or finding.

Rules for the governing thought:
- It must be a complete sentence (not "Cost reduction opportunities")
- It must be specific enough to be falsifiable ("Three actions can recover $40M by Q3" not "There are savings opportunities")
- It must be the logical conclusion of all the arguments below it

If the user hasn't stated a clear position yet, offer 2–3 governing thought options and ask them to choose or refine.

## Step 2 — Apply the SCR Frame (internal thinking only)

Use SCR to pressure-test the narrative logic — this is a thinking tool, not a labeling system for slides. Do not surface these labels to the audience.

- **Situation**: What is the accepted context? (1–2 sentences the audience already agrees with)
- **Complication**: What is the tension or problem that makes the governing thought necessary? (1–2 sentences)
- **Resolution**: Restate the governing thought as the answer to the complication. (1 sentence)

## Step 3 — Build Level 2 Arguments

Identify 3 (ideal) to 5 (maximum) arguments that together prove the governing thought.

For each argument:
- Write it as a complete sentence expressing an insight
- Verify it directly supports the governing thought (not just related to the topic)
- Apply the MECE check: are the arguments mutually exclusive? Together, do they fully prove Level 1?

If arguments overlap or leave gaps, restructure before proceeding.

## Step 4 — Map Supporting Evidence (Level 3)

For each Level 2 argument, list:
- The data points, analyses, or facts that prove it
- The source or analysis required (if data is not yet available, mark as [TBD])

Flag any Level 2 arguments that are currently unsupported — these represent analytical gaps.

## Step 5 — Check Horizontal Logic

Read only the Level 2 arguments in sequence. Ask:
- Does each argument flow logically from the previous one?
- Do they collectively build to the governing thought?
- Could a skeptical reader accept each argument individually?

If the horizontal logic is weak, consider reordering or reframing the arguments.

## Step 6 — Output

Deliver the structured argument as:

```
GOVERNING THOUGHT
[One sentence — the full recommendation]

NARRATIVE LOGIC (internal — do not label slides with these terms)
Situation: [1–2 sentences]
Complication: [1–2 sentences]
Resolution: [= governing thought]

PYRAMID STRUCTURE
1. [Level 2 Argument 1]
   - [Evidence / analysis]
   - [Evidence / analysis]

2. [Level 2 Argument 2]
   - [Evidence / analysis]
   - [Evidence / analysis]

3. [Level 2 Argument 3]
   - [Evidence / analysis]
   - [Evidence / analysis]

PROPOSED SLIDE ACTION TITLES
Slide 1: [Title slide]
Slide 2: [Executive Summary = Governing Thought]
Slide 3: [Action title — context/situation content]
...
```

Ask the user to confirm or adjust the structure before using it to build a deck with `/build-deck`.
