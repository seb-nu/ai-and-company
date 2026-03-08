---
description: First-time setup — configure your company template, branding, and preferences
allowed-tools: Read, Write, Bash
argument-hint: (no arguments needed)
---

You are running the AI&Company setup wizard. This command configures your personal settings **outside** the plugin folder so they survive plugin updates.

**Config will be stored at:** `~/.claude/ai-and-company/`

---

## Step 0 — Prepare Config Directory

Run this command silently before anything else:
```bash
mkdir -p ~/.claude/ai-and-company
```

Then greet the user:

> "Welcome to AI&Company setup. I'll walk you through 5 short steps to configure your company identity, PowerPoint template, and branding. Your settings will be saved to `~/.claude/ai-and-company/` and will not be affected by future plugin updates.
>
> Let's start."

---

## Step 1 — Company Identity

Ask the user these questions **in a single message**:

1. What is your company name?
2. What industry or sector are you in? *(e.g. Financial services, Healthcare, Consumer goods — helps Claude contextualise examples and terminology)*
3. What language should slide content be written in? *(default: English US)*
4. Do you have any terminology preferences? *(e.g. always "clients" not "customers", "FY24" not "fiscal year 2024", avoid "leverage" as a verb — or leave blank)*
5. What tone should Claude use? *(e.g. Formal executive, Direct and assertive, Conservative and measured)*

**Wait for answers before continuing.**

Store the answers internally — they will be written to `USER_CONFIG.md` in Step 6.

---

## Step 2 — PowerPoint Template

Tell the user:

> "Now let's set up your PowerPoint template.
>
> **Important:** Your template file must be an **empty PPTX** — no content slides, only a configured Slide Master. All colors, fonts, logos, and footer rules will be picked up automatically from the Slide Master layouts. If your current template has slides in it, delete them first and save, then upload.
>
> Please provide the full file path to your PPTX template file."

Wait for the user to provide the file path.

Once received:
- Verify the file exists using the Read tool (attempt to read the first bytes, or check via Bash: `ls "[path]"`)
- Copy the file to the config folder:
  ```bash
  cp "[user_provided_path]" ~/.claude/ai-and-company/template.pptx
  ```
- Confirm: "Template copied to `~/.claude/ai-and-company/template.pptx`. Running extraction now..."

---

## Step 3 — Extract Template Metadata

Invoke the `anthropic-skills:pptx` skill to run the following Python extraction script. Use the copied template at `~/.claude/ai-and-company/template.pptx`.

```python
from pptx import Presentation
from pptx.util import Pt
from pptx.dml.color import RGBColor
from lxml import etree
import json, os

TEMPLATE_PATH = os.path.expanduser("~/.claude/ai-and-company/template.pptx")
prs = Presentation(TEMPLATE_PATH)

# --- Canvas size ---
canvas_w = round(prs.slide_width.emu / 914400, 3)
canvas_h = round(prs.slide_height.emu / 914400, 3)

# --- Slide layouts ---
layouts = []
for i, layout in enumerate(prs.slide_layouts):
    placeholders = []
    for ph in layout.placeholders:
        fmt = ph.placeholder_format
        placeholders.append({
            "idx": fmt.idx,
            "type": str(fmt.type).split(".")[-1],  # e.g. TITLE, BODY, FOOTER
            "name": ph.name,
            "left_in":   round(ph.left   / 914400, 3),
            "top_in":    round(ph.top    / 914400, 3),
            "width_in":  round(ph.width  / 914400, 3),
            "height_in": round(ph.height / 914400, 3),
        })
    layouts.append({
        "index": i,
        "name": layout.name,
        "placeholder_count": len(placeholders),
        "placeholders": placeholders,
        "purpose": None,   # filled in after user validation
    })

# --- Slide master footer + slide number ---
master = prs.slide_master
footer_text = None
has_slide_number = False
slide_number_format = None   # "n" or "n/N" — inferred from XML shape names/types

for ph in master.placeholders:
    fmt = ph.placeholder_format
    type_str = str(fmt.type).split(".")[-1]
    if type_str == "FOOTER":
        try:
            footer_text = ph.text_frame.text.strip() or None
        except Exception:
            footer_text = None
    if type_str == "SLIDE_NUMBER":
        has_slide_number = True
        # Check if total-pages marker exists in the master XML
        nsmap = {"a": "http://schemas.openxmlformats.org/drawingml/2006/main",
                 "p": "http://schemas.openxmlformats.org/presentationml/2006/main"}
        xml_str = etree.tostring(master._element, encoding="unicode")
        if "sldNum" in xml_str and "numPt" in xml_str:
            slide_number_format = "n/N"
        else:
            slide_number_format = "n"

# --- Theme colors (from slide master theme XML) ---
theme_colors = {}
try:
    theme = master.theme_color_map  # may not be directly accessible
    # Fallback: parse the theme XML directly
    theme_part = master.part.part_related_by(
        "http://schemas.openxmlformats.org/officeDocument/2006/relationships/theme"
    )
    tree = etree.fromstring(theme_part.blob)
    ns = "http://schemas.openxmlformats.org/drawingml/2006/main"
    scheme = tree.find(f".//{{{ns}}}clrScheme")
    if scheme is not None:
        roles = ["dk1","lt1","dk2","lt2","accent1","accent2","accent3","accent4","accent5","accent6"]
        for role in roles:
            el = scheme.find(f"{{{ns}}}{role}")
            if el is not None:
                child = list(el)[0]
                tag = child.tag.split("}")[-1]
                if tag == "srgbClr":
                    theme_colors[role] = "#" + child.get("val", "")
                elif tag == "sysClr":
                    theme_colors[role] = "#" + child.get("lastClr", "")
except Exception as e:
    theme_colors["_error"] = str(e)

manifest = {
    "canvas": {"width_in": canvas_w, "height_in": canvas_h},
    "layouts": layouts,
    "theme_colors": theme_colors,
    "footer": {
        "confidentiality_label": footer_text,
        "has_slide_number": has_slide_number,
        "slide_number_format": slide_number_format,
    }
}

# Print JSON for Claude to read
print(json.dumps(manifest, indent=2))
```

Parse the JSON output. If the script errors, show the error to the user and ask them to verify the PPTX path and that python-pptx is installed.

---

## Step 4 — Bulk Validation

Present the following in one single message, then wait for user response:

**Layouts found** (formatted as a table):

| Index | Name | Placeholders | Suggested purpose |
|-------|------|-------------|-------------------|
| … | … | … | *(infer from name + placeholder types: TITLE+SUBTITLE → "Cover slide", TITLE+BODY → "Single-column content", TITLE+2×BODY → "Two-column content", etc.)* |

**Footer / numbering detected:**

> - Confidentiality label: *"[extracted text]"* (or "none found")
> - Slide numbering: `[n]` or `[n / N]` (or "none found")

**Questions to the user:**

1. "Does the layout mapping look right? Please correct any purpose labels or confirm with 'yes'."
2. "The confidentiality label I detected is: *[text]*. Do you want to **keep it**, **change it**, or **remove it**?"
3. "The slide numbering format appears to be `[format]`. Is that correct, or do you want a different format?"

Once the user responds, update the manifest layouts with confirmed `"purpose"` values, and store the confirmed footer/numbering values for writing in Step 6.

---

## Step 5 — Theme Colors

Display the colors extracted from the template (from the manifest's `theme_colors`), formatted as a table:

| Role | Hex | Mapped to |
|------|-----|-----------|
| dk1 | #000000 | Primary text |
| lt1 | #FFFFFF | Background |
| accent1 | … | Accent 1 |
| … | … | … |

Then ask the user (in one message) to fill in the chart-specific colors that are not captured in the theme XML:

> "I've extracted your theme colors above. I still need a few more that aren't stored in the theme:
>
> 1. **Primary chart color** (main data series) — e.g. `#003566`
> 2. **Secondary chart color** (comparison / secondary series)
> 3. **Positive / good indicator** — e.g. green `#2DC653`
> 4. **Negative / problem indicator** — e.g. red `#E63946`
> 5. **Callout / highlight box color** — e.g. yellow `#FFF3B0` or dark blue
>
> You can leave any of these blank to use MBB defaults."

Wait for answers.

---

## Step 6 — Write Config Files

Write the following two files using the Write tool.

### `~/.claude/ai-and-company/USER_CONFIG.md`

```markdown
# User Configuration
_Auto-generated by /setup. Edit directly or re-run /setup to update._
_This file lives outside the plugin and will not be affected by plugin updates._

---

## Company & Branding

**Company name:** [Step 1 answer]

**Industry / sector:** [Step 1 answer]

---

## PowerPoint Template

**Template file:** `~/.claude/ai-and-company/template.pptx`
**Template reference:** `~/.claude/ai-and-company/TEMPLATE.md`

_Layout purposes, placeholder positions, and theme colors are defined in TEMPLATE.md. Commands load it directly with `@~/.claude/ai-and-company/TEMPLATE.md`._

---

## PowerPoint Theme

_Colors extracted automatically from your template. Override any value below._

**Background:** [dk1/lt1 from theme_colors]
**Primary text:** [dk2 or dk1]
**Accent 1:** [accent1]
**Accent 2:** [accent2]
**Accent 3:** [accent3]
**Accent 4:** [accent4]

**Font — titles:** [extracted from theme font scheme, or "see template"]
**Font — body:** [extracted from theme font scheme, or "see template"]

---

## Chart & Callout Colors

**Primary chart color:** [Step 5 answer or blank]
**Secondary chart color:** [Step 5 answer or blank]
**Positive / good indicator:** [Step 5 answer or blank]
**Negative / problem indicator:** [Step 5 answer or blank]
**Callout / highlight box color:** [Step 5 answer or blank]

---

## Language & Style Preferences

**Output language:** [Step 1 answer]
**Terminology preferences:** [Step 1 answer or blank]
**Tone:** [Step 1 answer]

---

## Slide Standards

**Confidentiality label for footer:** [Step 4 confirmed value or blank]
**Numbering format:** [Step 4 confirmed value]
```

### `~/.claude/ai-and-company/TEMPLATE.md`

Write this file using the Write tool, filled in from the extraction and validation steps:

```markdown
# Company Template Reference
_Auto-generated by /setup from [template filename]. Re-run /setup to update._

---

## Canvas

**Width:** [W]"  **Height:** [H]"

---

## Slide Layouts

| Index | Layout Name | Purpose | Key placeholders |
|-------|-------------|---------|-----------------|
| 0 | [name] | [confirmed purpose, e.g. "cover"] | title (idx 0), subtitle (idx 1) |
| 1 | [name] | [confirmed purpose, e.g. "1-column"] | title (idx 0), body (idx 1) |
| … | … | … | … |

_Purpose values used by commands: `cover`, `1-column`, `2-column`, `3-column`, `last-page`, or any label the user confirmed._

---

## Placeholder Positions

### Layout [N] — [Name] ([purpose])

| Placeholder | idx | Left | Top | Width | Height |
|-------------|-----|------|-----|-------|--------|
| Title | 0 | [x]" | [y]" | [w]" | [h]" |
| Body | 1 | [x]" | [y]" | [w]" | [h]" |

_(Repeat block for each layout)_

---

## Footer

**Confidentiality label:** [confirmed text, or "none"]
**Slide numbering format:** [e.g. `n` or `n / N`, or "none"]

---

## Theme Colors

| Role | Hex | Maps to |
|------|-----|---------|
| dk1 | [hex] | Primary text |
| lt1 | [hex] | Background |
| accent1 | [hex] | Accent 1 |
| accent2 | [hex] | Accent 2 |
| accent3 | [hex] | Accent 3 |
| accent4 | [hex] | Accent 4 |

**Font — titles:** [extracted or "see template"]
**Font — body:** [extracted or "see template"]
```

---

## Step 7 — Summary

Print a confirmation summary:

```
Setup complete. Your config has been saved to ~/.claude/ai-and-company/

  Template ......... template.pptx
  Canvas ........... [W]" × [H]"
  Layouts mapped ... [N] layouts
    [0 → purpose, 1 → purpose, …]

  Company .......... [name] | [industry]
  Language ......... [language]
  Tone ............. [tone]
  Footer label ..... [label or "none"]
  Slide numbering .. [format]

All /build-deck and /improve-slide commands will now use your template and preferences.
Run /setup again at any time to update your configuration.
```
