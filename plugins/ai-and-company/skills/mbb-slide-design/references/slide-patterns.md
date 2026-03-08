# Classic Consulting Slide Patterns

Detailed layout rules and ASCII mockups for 8 essential MBB slide types.
For visual reference, see the PNG images in `${CLAUDE_PLUGIN_ROOT}/skills/mbb-slide-design/references/examples/`.
You don't have to strictly follow this patterns. You can use them as a guide on how to design the,

---

## How to Read the ASCII Mockups

```
┌─────────────────────────────────────────────┐
│ ZONE LABEL (width × height in inches)       │
└─────────────────────────────────────────────┘
```
Slide dimensions: 10" wide × 5.625" tall (16:9)

---

## 1. Title Slide

**Purpose**: Cover slide. Sets tone, client, confidentiality.
**Key rule**: Dark background only. The title IS the governing thought — not the project name.

```
┌──────────────────────────────────────────────────┐
│▌ [dark/charcoal background — full bleed]         │
│▌                                                 │
│▌  CLIENT NAME — PROJECT LABEL        [10pt caps] │
│▌                                                 │
│▌  [Main Title: The Full Governing Thought        │
│▌   in 2–3 lines, 36pt bold white]                │
│▌                                                 │
│▌  ━━━━━━━━━ [thin burnt-orange accent line]      │
│▌  Month Year  |  Confidential         [12pt]     │
│▌                                                 │
│▌  [Confidential label — bottom]       [9pt gray] │
└──────────────────────────────────────────────────┘
  ↑ Left burnt-orange bar (#C15F3C): 0.12" wide, full height
```

**Rules:**
- Title = governing thought sentence, not "Q1 Strategy Review"
- No bullets, no charts, no data
- Confidentiality statement always present

---

## 2. Executive Summary

**Purpose**: The full pyramid on one slide. Reader should be able to skip the rest.
**Key rule**: Title = governing thought (1 complete sentence). Three equal boxes = 3 supporting arguments.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: governing thought sentence]  22pt black   │
│  ─────────────────────────────────────────── [burnt-orange line] │
│                                                          │
│  ┌───────────────┐  ┌───────────────┐  ┌───────────────┐ │
│  │[▬ Dark hdr] ││  │[▬ Dark hdr] ││  │[▬ Dark hdr]  │ │
│  │  Argument 1  ││  │  Argument 2  ││  │  Argument 3   │ │
│  │               │  │               │  │               │ │
│  │ • evidence 1  │  │ • evidence 1  │  │ • evidence 1  │ │
│  │ • evidence 2  │  │ • evidence 2  │  │ • evidence 2  │ │
│  │ • evidence 3  │  │ • evidence 3  │  │ • evidence 3  │ │
│  └───────────────┘  └───────────────┘  └───────────────┘ │
│                                                          │
│  ─────────── [footer: confidential | project | slide #]  │
└──────────────────────────────────────────────────────────┘
  Box width: ~2.9" each | Box height: ~4.1" | Gap: ~0.1"
```

**Rules:**
- Exactly 3 boxes (3 arguments), always ideally for but can strech to 4/5
- Each box header = argument sentence, not a topic
- Each bullet = evidence, data point, or fact
- No chart, no visual — the structure IS the visual

---

## 3. Three-Pillar Framework

**Purpose**: Show three parallel, equal components of a solution or analysis.
**Key rule**: Pillars must be MECE. Headers are insight sentences, not labels.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title]                              22pt black   │
│  ─────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐  │
│  │▬▬▬▬▬▬▬▬▬▬▬▬│    │▬▬▬▬▬▬▬▬▬▬▬▬│    │▬▬▬▬▬▬▬▬▬▬▬▬│  │
│  │   [  01  ]  │    │   [  02  ]  │    │   [  03  ]  │  │
│  │             │    │             │    │             │  │
│  │  PILLAR     │    │  PILLAR     │    │  PILLAR     │  │
│  │  HEADER     │    │  HEADER     │    │  HEADER     │  │
│  │  ─────────  │    │  ─────────  │    │  ─────────  │  │
│  │ • point 1   │    │ • point 1   │    │ • point 1   │  │
│  │ • point 2   │    │ • point 2   │    │ • point 2   │  │
│  │ • point 3   │    │ • point 3   │    │ • point 3   │  │
│  └─────────────┘    └─────────────┘    └─────────────┘  │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Burnt-orange top accent bar: full width of card, 0.08" tall
  Large number (01/02/03): 32pt, warm gray (#B1ADA1), centered
  Card width: ~2.8" each
```

**Rules:**
- Pillars must be at the same level of abstraction
- Apply MECE: do they cover the full space without overlap?
- 3 bullets per pillar max — if more, the pillar is too broad

---

## 4. 2×2 Matrix

**Purpose**: Plot entities on two dimensions to reveal positioning insight.
**Key rule**: Axis labels are questions, not descriptions. Quadrant names are actions.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: insight about positioning]   22pt black   │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│         ┌─────────────┬─────────────┐                    │
│    ↑    │  MAINTAIN   │  ACCELERATE │                    │
│    |    │  (yellow bg)│  (green bg) │                    │
│   [Y    │    ⬤ BU-A  │   ⬤ BU-B    │                    │
│  AXIS   ├─────────────┼─────────────┤                    │
│  LABEL] │    EXIT     │    BUILD    │                    │
│    |    │  (red bg)   │  (dark gray bg)  │               │
│    ↓    │    ⬤ BU-D  │   ⬤ BU-C    │                    │
│         └─────────────┴─────────────┘                    │
|       Low ←── [X AXIS LABEL] ──→ High                    | 
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Matrix: ~7" wide × 4" tall, centered on slide
  Quadrant colors: yellow (harvest), green (invest), red (exit), dark gray (build)
  Entity circles: black, labeled below
```

**Rules:**
- Axis labels must be genuinely independent dimensions
- Quadrant names = strategic actions (Invest / Harvest / Exit / Build)
- Every entity plotted must be defensible — not decorative
- Max 2 entities per quadrant for readability

---

## 5. Bar / Column Chart Slide

**Purpose**: Show comparison, ranking, or trend in data with a clear insight.
**Key rule**: Chart title = the insight. The callout box directs the eye to the key point.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: insight from the data]       22pt black   │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          |
|                                                          │
│  ┌────────────────────────────────────┐  ┌────────────┐  │
│  │ [legend]                           │  │            │  │
│  │  25% ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─[bar]  │  |  • act 1   │  |
│  │  20% ─ ─ ─ ─ ─ ─ ─ ─ [bar]─ [bar]  │  |  • act 2   │  |
│  │  15% ─ ─ ─ ─ [bar] ─ [bar]─ [bar]  │  |  • act 3   │  │
│  │  10% ─[bar]─ [bar] ─ [bar]─ [bar]  │  └────────────┘  │
│  │   5% ─[bar]─ [bar] ─ [bar]─ [bar]  │                  │
│  │   0% ┴─────┴───── ┴──────┴───────│ |                  │
│  │       FY20  FY21   FY22  FY23      │                  │
│  │  ■ Company  ■ Market          [legend]                │
│  └────────────────────────────────────┘                  │
│  Source: [data source, year]              [8pt italic]   │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Chart: ~7.5" wide × 4.1" tall
  Callout box: ~1.65" wide × 1.3" tall, burnt-orange border (#C15F3C)
  Source: always present, bottom-left, 8pt italic
```

**Rules:**
- Chart title on the slide = the insight, NOT the chart's own axis title
- Max 2 data series per clustered bar chart
- Callout box must point to the specific bar/point that proves the title
- Source footnote is mandatory for all data slides

---

## 6. Process / Roadmap

**Purpose**: Show sequential phases of a plan with timing and activities.
**Key rule**: Phase names are verbs (Diagnose / Design / Implement), not nouns.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: insight about the journey]   22pt black   │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│  ┌────────┐  →  ┌────────┐  →  ┌────────┐  →  ┌────────┐ │
│  │ [●  1] │     │ [○  2] │     │ [○  3] │     │ [○  4] │ │
│  │DIAGNOSE│     │DESIGN  │     │IMPLEMENT│    │SUSTAIN │ │
│  │[timing]│     │[timing]│     │[timing] │    │[timing]│ │
│  │ ─────  │     │ ─────  │     │ ─────   │    │ ─────  │ │
│  │• act 1 │     │• act 1 │     │• act 1  │    │• act 1 │ │
│  │• act 2 │     │• act 2 │     │• act 2  │    │• act 2 │ │
│  │• act 3 │     │• act 3 │     │• act 3  │    │• act 3 │ │
│  └────────┘     └────────┘     └─────────┘    └────────┘ │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Active phase (1): black background, white text
  Future phases: light gray background, black text
  Phase circles: filled = active, outline = future
  Arrow connectors between phases: thin warm gray
```

**Rules:**
- First phase always highlighted (dark) — this is where we are now
- Timing shown under each phase name in italics
- Max 4 phases; 5 is acceptable but crowded
- Activities are parallel within a phase, sequential across phases

---

## 7. Waterfall / Bridge Chart

**Purpose**: Show how you get from a starting value to an ending value through components.
**Key rule**: Start and end bars are black. Positive drivers are green. Negative drivers are red.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: insight about the gap]       22pt black   │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│  $400M ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─[Target bar]─│   |
│  $300M ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─[+$75M]─ ─ ─ ─ ─ ─ ─ ─ │   |
│  $200M ─ ─ ─ ─ ─ ─ ─[+$60M] ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  │   |
│  $100M ─[start] ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─[+$45M] ─ ─ ─ │   |
│     $0 ┴───────────────────────────────────────────────  │
│         Current  Portfolio  Cost      Digital   Target   │
│         EBITDA   Optim.     Restruc.  Accel.    EBITDA   │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Start/End bars: black, full height from baseline
  Positive driver bars: dark green, floating above baseline
  Negative driver bars: red, hanging down from previous level
  Value labels: above each bar, bold, colored to match bar
```

**Rules:**
- Start and end bars always touch the baseline
- Floating bars show the delta, not the absolute value
- Label every bar with its dollar/percentage value
- Max 5–6 components; more becomes unreadable
- Add callout to explain drivers of the most meaningful variances

---

## 8. Recommendation Slide

**Purpose**: State the decision clearly and give the decision-maker everything needed to approve.
**Key rule**: The recommendation is in the title AND in a highlighted box. The three support boxes answer Why / How / What outcome.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: the recommendation]          22pt black   │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│  ┌────────────────────────────────────────────────────┐  │
│  │▌ RECOMMENDATION: [Full recommendation sentence     │  │
│  │▌  with decision, amount, and timeline]   13pt bold │  │
│  └────────────────────────────────────────────────────┘  │
│     ↑ Black background, burnt-orange left bar, white text │
│                                                          │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐   │
│  │[▬ Dark hdr] │    │[▬ Dark hdr] │    │[▬ Dark hdr] │   │
│  │  Why Now    │    │ How We Get  │    │  Expected   │   │
│  │             │    │   There     │    │  Outcomes   │   │
│  │ • reason 1  │    │ • step 1    │    │ • metric 1  │   │
│  │ • reason 2  │    │ • step 2    │    │ • metric 2  │   │
│  │ • reason 3  │    │ • step 3    │    │ • metric 3  │   │
│  └─────────────┘    └─────────────┘    └─────────────┘   │
│  [light gray next-steps bar at bottom]                   │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Recommendation box: black bg, burnt-orange left accent (#C15F3C), full width
  Three support boxes: equal width (~2.85"), light gray bg, dark header
  Next-steps bar: light gray, 1 line of text with immediate actions
```

**Rules:**
- Recommendation box restates the title as a complete, unambiguous instruction
- "Why Now / How / Outcome" is the standard 3-box structure for recommendation slides
- Next-steps bar shows the first 2–3 actions with owners and dates
- Never end a recommendation slide with "It depends" — make the call

---

## 9. Side-by-Side Bar Charts (Dual Action Subtitles)

**Purpose**: Compare two segments/scenarios with equal weight (e.g., Region A vs Region B, ) where each side needs its own insight headline.
**Key rule**: The slide has one governing action title, plus two *equal* columns. Each column’s subtitle is also an action title (a complete insight sentence), with a bar chart directly beneath it.

```
┌──────────────────────────────────────────────────────────┐
│  [Action Title: governing insight across both sides]     │
│  ──────────────────────────────────────── [burnt-orange line]    │
│                                                          │
│  [Action Subtitle (Left)]       [Action Subtitle (Right)]│
│  (complete insight sentence)  (complete insight sentence)│
│  ───────────────────────────   ───────────────────────── │
│  25% ─ ─ ─ ─ ─ ─ ─ ─ ─[bar]    25% ─ ─ ─ ─ ─ ─ ─[bar]    │
│  20% ─ ─ ─ ─ ─ ─ ─[bar]        20% ─ ─ ─ ─ ─[bar]        │
│  15% ─ ─ ─ ─ ─[bar]            15% ─ ─ ─ ─[bar]          │
│   0% ┴─────┴─────┴─────         0% ┴────┴────┴────       │
│       A     B     C                 A    B    C          │
│  Source: [data source, year]                [8pt italic] │
│  [footer]                                                │
└──────────────────────────────────────────────────────────┘
  Two open columns: equal width, aligned baselines and margins
  Subtitle style: 14–16pt bold black, left-aligned, 1–2 lines max
```

**Rules:**
- Both sides must be truly comparable — if one side is secondary, use a single main chart with a callout instead
- Keep the two panels equal in width and visual weight (same padding, same chart height)
- Use the same metric, units, and scale on both charts (shared axis range) unless the slide explicitly argues “scales differ”
- Align baselines, category order, and bar colors across both charts to make scanning effortless
- Each subtitle is an action title: a complete insight sentence, not a label (avoid “Region A”, write the takeaway)
- If you include a legend, use one shared legend for both panels to avoid duplication
- Source footnote is mandatory for all data slides
