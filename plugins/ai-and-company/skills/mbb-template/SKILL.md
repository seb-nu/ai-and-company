---
name: mbb-template
description: >
  Load this skill when the user's company configuration needs to be applied to
  slide output, or when a PowerPoint file needs to be generated with the correct
  template, theme colors, fonts, language, and branding.
  Covers: reading user config from ~/.claude/ai-and-company/, applying company
  palette and fonts, fallback to the AI&Company default .pptx template.
  Trigger phrases: "use my company colors", "apply my theme", "company template",
  "use our branding", "use the template", or any command that produces a .pptx file.
  Also trigger when the user invokes /build-deck or /improve-slide commands.
version: 0.1.0
---

# MBB Template & Company Configuration

This skill governs how the user's company identity and PowerPoint theme are applied to all slide output.
Load this skill whenever generating a `.pptx` file or applying company-specific settings.

For argument structure and logical frameworks, see the `mbb-thinking` skill.
For visual slide design standards, see the `mbb-slide-design` skill.

---

## User Config Folder

All personal configuration lives at **`~/.claude/ai-and-company/`**, created by the `/setup` command:

```
~/.claude/ai-and-company/
  USER_CONFIG.md   ← company identity, language, tone, colors
  template.pptx    ← user's custom empty PPTX template
  TEMPLATE.md      ← layout map: index → purpose, placeholder positions, theme colors
```

This folder lives outside the plugin and is never touched by plugin updates.

---

## Config Resolution

Check for user config by running: `ls ~/.claude/ai-and-company/USER_CONFIG.md 2>/dev/null`

**If `~/.claude/ai-and-company/USER_CONFIG.md` exists** — read it and apply all settings below.

**If it does not exist** — use MBB defaults (English, formal executive tone, AI&Company palette) and suggest: *"Run `/setup` to configure your company template and branding — it only takes a few minutes."*

---

## Company Configuration

When user config exists, apply these settings from `~/.claude/ai-and-company/USER_CONFIG.md`:

- **Output language** — write all slide content in the specified language
- **Terminology** — apply the user's preferred terms throughout (e.g. "clients" not "customers")
- **Tone** — match the user's specified tone (formal executive, direct, etc.)
- **Chart colors** — use the company's primary/secondary/positive/negative palette
- **Callout color** — use the company's callout box color for highlight boxes
- **Confidentiality label & numbering** — apply in slide footers
- **Theme fonts** — use the specified title and body fonts when building `.pptx` files

---

## Template Resolution

When generating a `.pptx` file, choose the base template as follows:

**User has run `/setup` and uploaded their company template:**
```
Presentation("~/.claude/ai-and-company/template.pptx")
```
Load `@~/.claude/ai-and-company/TEMPLATE.md` for the layout map — use the Purpose column to select the correct `slide_layouts[index]` for each slide type (cover, 1-column, 2-column, etc.).

Chart colors and callout colors come from `~/.claude/ai-and-company/USER_CONFIG.md`.

**No user template exists (setup not run, or user skipped template upload):**
```
Presentation("${CLAUDE_PLUGIN_ROOT}/skills/mbb-template/assets/AI&Company Template.pptx")
```
Use the hardcoded layout indexes in the Template Reference section below. Colors and fonts are baked into the AI&Company template.

> Check for the user template with: `ls ~/.claude/ai-and-company/template.pptx 2>/dev/null`

---

## Template Reference

The following is extracted from `assets/AI&Company Template.pptx` and applies whenever the default template is used.

### Color Palette (theme scheme: "AI&Company")

| Role | Hex | Usage |
|------|-----|-------|
| Primary brand orange | `#BF5C38` | Rule lines, cover heading, last-page line |
| Body text | `#000000` | All body text |
| Background | `#FFFFFF` | Slide background |
| Accent warm grey | `#B1ADA1` | Secondary fills (Accent1) |
| Accent terracotta | `#C15F3C` | Chart / data color (Accent3) |
| Light grey | `#DDDDDD` | Table borders, dividers (Accent4) |
| Mid grey | `#B2B2B2` | Secondary labels (Accent5) |
| Dark grey | `#606060` | Footer text (Accent6) |
| Slide number | `#000000` @ 82% tint | Bottom-right page number |

### Typography (font scheme: "Arial")

- **Title font**: Arial — 24pt, left-aligned, bottom-anchored
- **Body font**: Arial — bullet hierarchy: 16pt (L1) → 14pt (L2) → 12pt (L3) → 11pt (L4–L5)
- **Line spacing**: 90% throughout
- **Bullet character**: `•`

### Structural Master Elements

These elements are baked into the slide master and appear automatically on all non-blank slides:

- **Orange rule line** below title — `#BF5C38`, 1.5pt, full width
- **Grey footer rule** near bottom — `#999999`, 0.75pt, full width
- **Slide number** — 8pt, right-aligned, bottom-right corner
- **Confidentiality footer** — 6pt, `#606060`: *"This information is confidential and was prepared by AI & Company solely for the use of our client; it is not to be relied on by any 3rd party without AI & Company's prior consent"*
- **AI&Company logo** — embedded image, bottom-right of master

### Slide Layouts

Access via `prs.slide_layouts[n]` in python-pptx. **Slide canvas: 13.33" × 7.50"**

| Index | Name | Use for |
|-------|------|---------|
| 0 | Cover Slide | First slide / deck cover |
| 1 | 1 column slide | Standard narrative or single-chart slide |
| 2 | 2 column slide | 2 different contents under the same core ideas |
| 3 | 3 column slide | Three-option or pillar structure |
| 4 | Last Page Logo | Closing / thank-you slide |

### Source Placement (all layouts)

Add sources as an **unformatted text box** (not a placeholder) so it survives layout changes.

- **Position**: `left=0.38"  top=6.85"  w=12.58"  h=0.30"` — sits at the bottom of the body area, above the grey footer rule at `7.23"`
- **Style**: Arial 6pt, color `#606060`, left-aligned, no bullet
- **Format**: `Source: <Author / Dataset>, <Year>` — e.g. `Source: McKinsey Global Institute, 2024`
- **Omit entirely** if no source is needed (do not leave an empty box)

```python
from pptx.util import Inches, Pt
from pptx.dml.color import RGBColor
from pptx.enum.text import PP_ALIGN

txBox = slide.shapes.add_textbox(
    Inches(0.38), Inches(6.85), Inches(12.58), Inches(0.30)
)
tf = txBox.text_frame
tf.word_wrap = True
p = tf.paragraphs[0]
p.text = "Source: McKinsey Global Institute, 2024"
p.alignment = PP_ALIGN.LEFT
run = p.runs[0]
run.font.size = Pt(6)
run.font.color.rgb = RGBColor(0x60, 0x60, 0x60)
```

---

### Layout 0 — Cover Slide (`prs.slide_layouts[0]`)

No body content or source line — this is the title slide only.

| Element | idx | Left | Top | Width | Height | Notes |
|---------|-----|------|-----|-------|--------|-------|
| Title | 0 | 0.38" | 3.20" | 12.50" | 0.88" | Bold, `#BF5C38` brand orange |
| Subtitle | 1 | 0.38" | 4.24" | 12.50" | 0.71" | Color `#B1ADA1` warm grey |
| Date | 10 | 9.88" | 6.09" | 3.00" | 0.40" | Right-aligned |

---

### Layout 1 — 1 Column Slide (`prs.slide_layouts[1]`)

| Element | idx | Left | Top | Width | Height | Notes |
|---------|-----|------|-----|-------|--------|-------|
| Title | 0 | 0.38" | 0.09" | 12.58" | 0.88" | Arial 24pt, bottom-anchored |
| Content zone *(master)* | 1 | 0.38" | 1.42" | 12.58" | 5.74" | Text, chart, diagram, image — anything |
| Source *(text box)* | — | 0.38" | 6.85" | 12.58" | 0.30" | See Source Placement above |

**Content zone**: `left 0.38" → right 12.96"`, `top 1.42" → bottom ~6.80"` (keep bottom ~0.43" clear for source)
**Source goes**: `top 6.85"` (0.38" above grey footer rule at `7.23"`)

The content zone is a bounding region — place any shape (text box, chart, picture, SmartArt, table, etc.) within these bounds. For a text-only slide, use the inherited body placeholder (idx=1); for charts or diagrams, add them as shapes sized to fit the zone.

```python
layout = prs.slide_layouts[1]
slide  = prs.slides.add_slide(layout)
slide.shapes.title.text = "Slide Title"

# Text content — use inherited master placeholder (idx=1)
for ph in slide.placeholders:
    if ph.placeholder_format.idx == 1:
        ph.text = "Body content here"
        break

# Chart / image — add as a shape within the content zone bounds
# chart_data = ...
# slide.shapes.add_chart(chart_type, Inches(0.38), Inches(1.42), Inches(12.58), Inches(5.38), chart_data)
```

---

### Layout 2 — 2 Column Slide (`prs.slide_layouts[2]`)

| Element | idx | Left | Top | Width | Height | Notes |
|---------|-----|------|-----|-------|--------|-------|
| Title | 0 | 0.38" | 0.09" | 12.58" | 0.88" | |
| Left header | 11 | 0.38" | 1.42" | 6.00" | 0.40" | Column subtitle placeholder |
| Right header | 12 | 6.96" | 1.42" | 6.00" | 0.40" | Column subtitle placeholder |
| *(divider lines)* | — | 0.38" / 6.96" | 1.82" | 6.00" each | — | Black 1pt rule under each header |
| Left content zone | — | 0.38" | 1.90" | 6.00" | 4.90" | Text, chart, diagram, image, etc. |
| Right content zone | — | 6.96" | 1.90" | 6.00" | 4.90" | Text, chart, diagram, image, etc. |
| Source *(text box)* | — | 0.38" | 6.85" | 12.96" | 0.30" | See Source Placement above |

**Content zones**: `top 1.90" → bottom ~6.80"` per column — place any shape(s) within these bounds
**Gap between columns**: 0.58" (6.38" → 6.96") — acts as visual separator
**Source goes**: `top 6.85"` full width

```python
layout = prs.slide_layouts[2]
slide  = prs.slides.add_slide(layout)
slide.shapes.title.text = "Slide Title"

# Column headers via placeholder idx
for ph in slide.placeholders:
    if ph.placeholder_format.idx == 11:
        ph.text = "Left Column Header"
    elif ph.placeholder_format.idx == 12:
        ph.text = "Right Column Header"

# Content — add any shape(s) within each zone's bounds
# Text example:
left_body  = slide.shapes.add_textbox(Inches(0.38), Inches(1.90), Inches(6.00), Inches(4.90))
right_body = slide.shapes.add_textbox(Inches(6.96), Inches(1.90), Inches(6.00), Inches(4.90))
# Chart example (replace add_textbox with add_chart, sized to the same zone):
# slide.shapes.add_chart(chart_type, Inches(0.38), Inches(1.90), Inches(6.00), Inches(4.90), chart_data)
```

---

### Layout 3 — 3 Column Slide (`prs.slide_layouts[3]`)

| Element | idx | Left | Top | Width | Height | Notes |
|---------|-----|------|-----|-------|--------|-------|
| Title | 0 | 0.38" | 0.09" | 12.58" | 0.88" | |
| Left header | 11 | 0.38" | 1.42" | 3.90" | 0.40" | Column subtitle placeholder |
| Middle header | 12 | 4.72" | 1.42" | 3.90" | 0.40" | Column subtitle placeholder |
| Right header | 13 | 9.04" | 1.42" | 3.92" | 0.40" | Column subtitle placeholder |
| *(divider lines)* | — | 0.38" / 4.72" / 9.06" | 1.82" | 3.90" each | — | Black 1pt rule under each header |
| Left content zone | — | 0.38" | 1.90" | 3.90" | 4.90" | Text, chart, diagram, image, etc. |
| Middle content zone | — | 4.72" | 1.90" | 3.90" | 4.90" | Text, chart, diagram, image, etc. |
| Right content zone | — | 9.04" | 1.90" | 3.92" | 4.90" | Text, chart, diagram, image, etc. |
| Source *(text box)* | — | 0.38" | 6.85" | 12.96" | 0.30" | See Source Placement above |

**Content zones**: `top 1.90" → bottom ~6.80"` per column — place any shape(s) within these bounds
**Gaps between columns**: 0.44" (left→mid) and 0.42" (mid→right)
**Source goes**: `top 6.85"` full width

```python
layout = prs.slide_layouts[3]
slide  = prs.slides.add_slide(layout)
slide.shapes.title.text = "Slide Title"

for ph in slide.placeholders:
    if ph.placeholder_format.idx == 11:
        ph.text = "Left Header"
    elif ph.placeholder_format.idx == 12:
        ph.text = "Middle Header"
    elif ph.placeholder_format.idx == 13:
        ph.text = "Right Header"

# Content — add any shape(s) within each zone's bounds
# Text example:
left_body   = slide.shapes.add_textbox(Inches(0.38), Inches(1.90), Inches(3.90), Inches(4.90))
middle_body = slide.shapes.add_textbox(Inches(4.72), Inches(1.90), Inches(3.90), Inches(4.90))
right_body  = slide.shapes.add_textbox(Inches(9.04), Inches(1.90), Inches(3.92), Inches(4.90))
# Chart example (replace add_textbox with add_chart, sized to the same zone):
# slide.shapes.add_chart(chart_type, Inches(0.38), Inches(1.90), Inches(3.90), Inches(4.90), chart_data)
```

---

### Layout 4 — Last Page Logo (`prs.slide_layouts[4]`)

No editable content or source line — logo and wordmark only.

| Element | Left | Top | Width | Height | Notes |
|---------|------|-----|-------|--------|-------|
| Logo group | 8.46" | 5.22" | 4.50" | 0.74" | "AI & COMPANY" wordmark + logo |
| Orange rule | 0.00" | 6.00" | 12.96" | — | `#BF5C38`, no master chrome |
