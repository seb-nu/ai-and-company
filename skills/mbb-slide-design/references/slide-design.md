# Slide Design Standards — Detailed Reference

_Visual and formatting standards used in MBB consulting deliverables._

---

## Typography

| Element | Guideline |
|---|---|
| Slide title (action title) | 24pt, bold, one line if possible, two lines is ok |
| Body text | 16pt (L1) · 14pt (L2) · 12pt (L3) · 11pt (L4–L5) |
| Chart labels / annotations | 14–16pt |
| Footnotes / sources | 6pt |
| Font family | Arial (or client brand font) |

If text doesn't fit at 16pt, the slide has too much content — cut content, don't shrink font.

---

## Color

- **Primary palette**: 2–3 colors maximum (usually brand colors + one accent)
- **Data series**: Use distinct, colorblind-safe colors for chart series
- **Emphasis**: Use one accent color sparingly for the "so what" callout
- **Avoid**: Gradients, drop shadows, 3D effects, more than 4 colors in one chart

**AI&Company color conventions (brand palette):**
- Black (#000000) = primary structural color (text, borders, dark backgrounds)
- Warm gray (#B1ADA1) = secondary neutral (supporting fills, pillar numbers)
- Burnt orange (#C15F3C) = accent / emphasis / callout highlights
- Light gray (#DDDDDD) = supporting fills and dividers

**Data visualization semantic colors (chart-level only, not brand slots):**
- Green = positive driver / solution / recommendation
- Red = negative driver / warning / problem

---

## Chart Formatting Standards

### Always Include
- Chart title (expressing the insight, not the axis label)
- Axis labels with units
- Data source / footnote
- Clear legend (or direct labels on bars/lines — preferred)

### Never Include
- 3D effects on any chart type
- Gridlines heavier than 0.5pt
- More than 6 data series on one chart
- Pie charts (strongly discouraged — use stacked bar for part-to-whole; pie charts obscure magnitude comparison and should only be used if a client template explicitly requires them)
- Vertical / column charts (strongly discouraged — use horizontal bar charts; they are easier to read, allow longer category labels, and make ranking comparisons clearer)
- Dual Y-axes (almost always possible to split into two charts)

### Annotation Best Practice
Add a callout box or arrow pointing to the most important data point.
This guides the reader's eye to the insight the title is expressing.
Example: Bar chart with callout on the highest/lowest bar saying "This is the outlier driving the average up."

---

## Slide Layout Principles

### Whitespace
- Leave at least 10% whitespace around all content
- Content should not touch slide edges
- Breathing room between chart, title, and footer increases clarity

### Alignment
- All elements should align to an invisible grid
- Consistent left margin for all text elements
- Chart and text boxes should share a common bottom edge when side by side

### Content Column Structure (1 / 2 / 3 columns)

Every slide body is organised around **1, 2, or 3 content columns** beneath the action title.
The column count is a layout decision — not a content type. Columns can hold any mix of
content atoms: bullet arguments, charts, tables, callout boxes, timelines, icon+stat pairs.

**1 column — single unified content block**
Use when the insight is carried by one artefact (one big chart, one framework, one visual).
```
┌──────────────────────────────────────────────────────────┐
│  [Action Title]                              24pt black   │
│  ────────────────────────────────────────────────────    │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │  [single content block — chart, table, visual,   │   │
│  │   argument list, or framework — full width]       │   │
│  └──────────────────────────────────────────────────┘   │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
```

**2 columns — two equal panels, each with its own subtitle**
Use when contrasting or comparing two equally weighted ideas. Panels can carry
*different* content types (e.g., left = bullet arguments, right = bar chart).
```
┌──────────────────────────────────────────────────────────┐
│  [Action Title]                              24pt black   │
│  ────────────────────────────────────────────────────    │
│                                                          │
│  [Subtitle A — insight sentence]  [Subtitle B — insight] │
│  ──────────────────────────────   ───────────────────── │
│  ┌──────────────────────────┐     ┌─────────────────┐   │
│  │  content A               │     │  content B      │   │
│  │  (any type)              │     │  (any type)     │   │
│  └──────────────────────────┘     └─────────────────┘   │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
```

**3 columns — three equal panels, each with its own subtitle**
Use when proving three MECE components of the same argument. Same rule applies:
each panel can hold a different content type.
```
┌──────────────────────────────────────────────────────────┐
│  [Action Title]                              24pt black   │
│  ────────────────────────────────────────────────────    │
│                                                          │
│  [Subtitle A]       [Subtitle B]       [Subtitle C]      │
│  ─────────────      ─────────────      ─────────────     │
│  ┌───────────┐      ┌───────────┐      ┌───────────┐    │
│  │ content A │      │ content B │      │ content C │    │
│  │ (any type)│      │ (any type)│      │ (any type)│    │
│  └───────────┘      └───────────┘      └───────────┘    │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
```

**Valid content atoms for any column:**
bullet list · bar / line / waterfall chart · 2×2 matrix · table · icon + stat · timeline · callout box

**Decision rules:**
- 1 column: one artefact carries the full insight on its own
- 2 columns: two ideas need equal visual weight; often contrast or before/after
- 3 columns: three MECE sub-arguments support one governing thought
- Never force 3 columns on content that only warrants 2 — prefer clarity over symmetry
- Subtitles are only needed for 2- and 3-column layouts; they must be action sentences, not labels

---

### Footer
Standard MBB footer contains (left to right):
- Confidentiality label ("Confidential" or "Proprietary")
- Document name / version
- Slide number (e.g., "5" or "5/22")
- Date (month/year)

---

## Common Visual Mistakes

| Mistake | Fix |
|---|---|
| Chart and title say the same thing | Either make the title more insightful or the chart simpler |
| Too many colors in one chart | Use shades of one color; use color only for emphasis |
| Cluttered chart with 8+ series | Split into two charts or use a different chart type |
| Bullet points in a list of 8 | Group into 2–3 buckets with headers (MECE check) |
| Decorative icons | Remove; they add noise without information |
| Pie chart used for part-to-whole | Replace with a stacked bar chart; pie charts hide magnitude differences |
| Vertical / column chart used | Replace with horizontal bar chart; labels fit better and ranking reads naturally top-to-bottom |

---

## Backup / Appendix Standards

- Appendix slides follow the same formatting standards as main deck slides
- Each backup slide should have an action title
- Number appendix slides separately (A1, A2...) or continue main numbering
- Use a divider slide at the start: "Appendix" with a list of what's included
- Only move content to backup if you genuinely won't need it in the room
