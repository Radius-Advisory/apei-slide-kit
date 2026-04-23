---
name: apei-pptx
description: Apply these preferences whenever creating or editing a PowerPoint presentation for APEI (American Public Education, Inc.) or for Iam Williams working on APEI engagements. Read this file in addition to the standard pptx/SKILL.md. Triggers: any slide deck, board deck, investor deck, or .pptx request that references APEI, Iam, Angie, James K., James D., or William.
---

# APEI PPTX Skill

**Read this alongside `/mnt/skills/public/pptx/SKILL.md`.** These rules override or extend the defaults where they conflict.

This skill is for producing board-ready, executive-grade decks in APEI's brand. The writing voice should match top-tier strategy consulting firms (McKinsey, BCG, PwC, Bain): clear, direct, structured, and specific — never fluffy, never buzzword-laden.

---

## Pre-Flight: Get the Template

Before starting any APEI slide work, you need the APEI brand template file. Try these sources in order:

### Source 1: Auto-fetch from GitHub (default — no user action needed)

```bash
curl -L -f -o template.pptx \
  https://raw.githubusercontent.com/Radius-Advisory/apei-slide-kit/main/templates/APEI_sanitized_ppttemplate.pptx
```

If `curl` exits 0 and the file is non-empty, proceed. This is the sanitized public template — always current, always brand-compliant.

> **Setup note (one-time, for the skill owner):** Replace `Radius-Advisory` above with the actual GitHub account hosting the repo. Everything else in the skill assumes this URL works.

### Source 2: User-uploaded file

If the GitHub fetch fails (no network, repo moved, etc.), check the conversation for an uploaded template:

```bash
ls /mnt/user-data/uploads/ 2>/dev/null | grep -i apei
```

If a file named like `APEI_sanitized_ppttemplate*.pptx` exists there, use it.

### Source 3: Ask the user

If both sources above fail, stop and respond with:

> I couldn't auto-fetch the APEI template from GitHub and don't see one uploaded. Please either:
>
> 1. Upload the template file (e.g. `APEI_sanitized_ppttemplate.pptx`) to this conversation, or
> 2. Download it from APEI's Brandfolder (<https://brandfolder.com/s/9thk4vrzm8jhfmrjqwgpf8b5>) and upload it here.
>
> Once I have the template, I'll build the slides using its exact layouts.

**Do not proceed without a template. Do not rebuild layouts from scratch.**

### Standard PPTX Skill

This skill also depends on the standard pptx tooling in `/mnt/skills/public/pptx/` (`unpack.py`, `pack.py`, `clean.py`, `add_slide.py`, `thumbnail.py`, `extract-text`). If those scripts are missing, ask the user to enable the standard PPTX skill before proceeding.

---

## Core Principle: Template-First, Not From-Scratch

**Always start from the real template slides. Never fabricate layouts that "look like" the template.**

The APEI template has ~38 professionally designed slides covering every layout APEI needs (cover, agenda, section dividers, stat callouts, team bios, process timelines, charts, maps, closing slides, and a full icon library). Use them as starting points.

### Workflow

```
1. Fetch template (see Pre-Flight above)

2. Analyze
   python /mnt/skills/public/pptx/scripts/thumbnail.py template.pptx
   extract-text template.pptx

3. Plan slide mapping
   For each content section in the user's request, pick the best-matching template slide.
   Prefer varied layouts — don't reuse slide 7 twelve times.

4. Unpack
   python /mnt/skills/public/pptx/scripts/office/unpack.py template.pptx unpacked/

5. Build structure (BEFORE editing content)
   - Duplicate template slides you'll reuse (add_slide.py)
   - Delete unneeded slides from <p:sldIdLst>
   - Reorder slides in <p:sldIdLst>

6. Edit content
   Use the Edit tool on each slide{N}.xml. Replace placeholder text and images with real content.
   Keep the existing shape/text-box structure intact.

7. Clean + Pack
   python /mnt/skills/public/pptx/scripts/clean.py unpacked/
   python /mnt/skills/public/pptx/scripts/office/pack.py unpacked/ output.pptx --original template.pptx

8. Visual QA
   Convert to PDF → pdftoppm → inspect each slide with a subagent.
```

### When PptxGenJS Is Appropriate (Rare)

Use the programmatic PptxGenJS approach **only** when:
- The user explicitly requests a custom layout the template doesn't support, AND
- The user has approved deviating from the template

Even then, match the brand system (colors, fonts, corner mark, footer treatment) exactly.

---

## APEI Brand System

### Primary Color Palette (Exact Values)

| Role | Name | Hex | RGB | Pantone |
|------|------|--------|---------------|---------|
| **Primary dark** | APEI Navy | `#22314E` | 34 / 49 / 78 | 533 |
| **Primary accent** | APEI Green | `#CCDC00` | 204 / 220 / 0 | 389 |
| **Neutral** | APEI Gray | `#E5E5E5` | 229 / 229 / 229 | — |
| **Secondary accent** | APEI Blue | `#0072CF` | 0 / 114 / 207 | — |

**Usage hierarchy:**
- **APEI Navy** — primary brand color; use for title slide background, closing slides (Questions / Thank You), section divider backgrounds, section header text ("SECTION TITLE"), headline typography on light backgrounds, and the upper-right stacked corner-mark triangles.
- **APEI Green** — sharp accent; use for the corner-mark triangles, agenda numbering, small underline accents beneath titles, key data highlights, and callout bars. Use sparingly for emphasis.
- **APEI Gray** — neutral; use for dividers, light section backgrounds, de-emphasized text, table alternating rows, and subtle fills.
- **APEI Blue** — secondary accent for charts, data viz, and map highlights; also used in some infographic slides alongside navy and green.

### Extended Palette (When More Colors Are Needed)

When a chart, infographic, or diagram requires more than the four primaries, use colors that sit naturally next to APEI Navy + Green + Blue:

| Purpose | Name | Hex |
|---------|------|--------|
| Darker navy shade | Deep Navy | `#0F1A2E` |
| Mid-navy (charts) | Steel Blue | `#3E5373` |
| Light navy (backgrounds) | Mist | `#E8ECF3` |
| Warm gray (muted text) | Slate Gray | `#6B7280` |
| Chart neutral | Chart Gray | `#B8BEC7` |
| Soft green (data rows) | Muted Lime | `#E8EE99` |
| White | White | `#FFFFFF` |
| Near-black (body text) | Near Black | `#1A1A1A` |

**Rules for extended colors:**
- Never introduce a new hue (red, orange, purple, teal) without an explicit reason and user approval
- Tints and shades of the primaries are always safe
- If you need a warning/error tone, ask the user before using red; APEI's decks generally avoid alarm colors

### Typography — Open Sans Family

**All text uses Open Sans.** This is enforced by the template and should not be swapped.

| Weight | PPTX typeface string | Use |
|--------|---------------------|------|
| Open Sans Light | `Open Sans Light` | Large titles, hero numbers, elegant headlines |
| Open Sans | `Open Sans` | Body text, captions, labels |
| Open Sans Bold (applied via `b="1"`) | `Open Sans` + bold | Emphasis, small caps section labels, table headers |

If Open Sans is not available on the rendering machine, the template falls back through its theme font. **Never override to Calibri or Arial in new content** — that breaks brand consistency.

**Type scale (starting points, tune per layout):**

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| Cover title | 44–54 pt | Light | APEI Green or White |
| Cover subtitle | 16–20 pt | Regular | White |
| Section divider title | 44–60 pt | Light/Regular | Navy or White |
| Slide title | 32–40 pt | Light/Regular | APEI Navy |
| "SECTION TITLE" eyebrow | 10–11 pt, tracked/caps | Bold | APEI Navy |
| Subtitle / tagline | 14–16 pt | Regular | APEI Navy or Slate Gray |
| Body | 11–13 pt | Regular | Near Black or APEI Navy |
| Large stat number | 48–72 pt | Light | APEI Navy or APEI Green |
| Caption / footnote | 8–9 pt | Regular | Slate Gray |
| Footer (page number + company) | 7–8 pt | Regular | Slate Gray |

### Signature Brand Elements

These must appear on every content slide (they're already in the template — don't remove them when editing):

1. **Stacked right-triangle corner mark (top right)** — two overlapping triangles, one APEI Green + one APEI Navy. This is the most distinctive APEI visual mark.
2. **Short green underline accent** beneath the slide title on content slides.
3. **"SECTION TITLE" eyebrow** in small all-caps navy above the slide title.
4. **Footer** with APEI logo-lockup "American Public Education, Inc." on closing slides; small page number on content slides.
5. **Cover/closing slides** use a navy background with green accents (triangles, underline under "Questions?", etc.).

---

## Icons: Use Template Icons Only

The template includes a dedicated icon library (typically slides 37 and 38) with 80+ line-style icons in two variants:
- **Slide 37** — navy outline icons on white
- **Slide 38** — white outline icons on APEI Blue

**Rules:**

1. **Always reuse icons from the template's icon library.** Do not generate new icons, do not import icons from Lucide/Heroicons/Font Awesome, do not use emoji as icons.
2. **If the template icon library doesn't have what you need**, the acceptable fallbacks are (in order):
   - Simplify the concept to use an icon that does exist
   - Use an SVG in the same visual style as the template icons (thin line, ~1.5pt stroke, monochrome, rounded joins) — stroke color must be APEI Navy, APEI Green, APEI Blue, or white
   - Ask the user before introducing any external icon
3. **Never mix icon styles on the same slide.** If two icons come from different style families, replace one.
4. To use a template icon on a new slide, copy the shape/image element (and its relationship) from the icon slide into the destination slide — do not rasterize.

---

## Writing Style: Top-Tier Consulting Voice

All text should read like a McKinsey, BCG, or PwC deliverable. That means:

### Structural principles

- **Pyramid principle.** Lead with the answer. Then give 2–4 supporting points. Details come last.
- **MECE content.** Bullets should be mutually exclusive and collectively exhaustive. No overlapping ideas.
- **Rule of three.** When possible, use three pillars, three takeaways, three phases. Three is memorable; four is a list; two is a contrast.
- **Action-oriented titles.** The slide title should state the insight, not the topic.
  - ❌ "Financial Aid Overview"
  - ✅ "Financial aid processing drives 60% of student support volume"

### Language principles

- **Specific over vague.** "Reduces cycle time by ~30%" not "significantly improves efficiency."
- **Nouns and verbs, not adjectives.** "Launched the pilot in Q2" not "successfully launched a robust pilot in Q2."
- **Short sentences.** 15–20 words maximum for body copy. Split anything longer.
- **Parallel structure in bullets.** Every bullet in a group starts with the same part of speech and has similar length.
- **No filler.** Delete "in order to," "it is important to note that," "as we all know," "going forward."
- **American English.** "Organization," "realize," "specialize," "while" (not "whilst"), single-L past tenses ("enrolled," "canceled"), month-day-year dates.
- **Numbers with context.** Never show a number without the comparison that makes it meaningful ("$27M — up 12% YoY" not just "$27M").
- **No buzzword stacking.** Avoid phrases like "leverage synergies," "digital transformation journey," "best-in-class solutions," "paradigm shift."

### Title hierarchy on a content slide

```
SECTION TITLE              ← 10pt navy bold tracked caps (eyebrow)
Write the insight here     ← 32–40pt navy light (action title)
——                         ← short green underline accent
Supporting subtitle        ← 14pt navy regular
[Body / bullets / viz]     ← 11–13pt
```

### Bullet discipline

- Max 5 bullets per slide. If you have more, either (a) split into two slides or (b) group into 3 themes with 1–2 lines each.
- Max 2 lines per bullet. If a bullet wraps to 3 lines, cut it or split it.
- Parallel: every bullet starts with the same word-class (all verbs, or all noun phrases — not a mix).

---

## Text Boxes: Consolidate, Don't Scatter

**Rule: Related content goes in one text box, not many.**

A slide section with a header + 4 bullets = 1 `addText()` call using a rich text array with `breakLine: true`, not 5 separate calls. Use mixed formatting within a single text box (bold header, normal body, bullet items) rather than stacking independent boxes.

**Exception:** truly independent layout elements (e.g., a stat callout in the corner separate from the main body) may be their own box.

```javascript
// CORRECT - one editable block
slide.addText([
  { text: "Section Header", options: { bold: true, fontSize: 16, breakLine: true } },
  { text: "First point", options: { bullet: true, fontSize: 13, breakLine: true } },
  { text: "Second point", options: { bullet: true, fontSize: 13, breakLine: true } },
  { text: "Third point", options: { bullet: true, fontSize: 13 } }
], { x: 0.5, y: 1.2, w: 4.5, h: 2.5, fontFace: "Open Sans" });

// WRONG - hard to edit, cluttered object panel
slide.addText("Section Header", { x: 0.5, y: 1.2, w: 4.5, h: 0.4, bold: true });
slide.addText("First point", { x: 0.5, y: 1.7, w: 4.5, h: 0.35 });
slide.addText("Second point", { x: 0.5, y: 2.1, w: 4.5, h: 0.35 });
slide.addText("Third point", { x: 0.5, y: 2.5, w: 4.5, h: 0.35 });
```

When editing template XML directly, the same principle applies: if the template already has one text box holding a header + bullets, keep them together in one `<p:sp>` with multiple `<a:p>` paragraphs — don't split them into separate shapes.

---

## Tables: Use Real PowerPoint Tables

**Rule: Never fake a table with shapes + text boxes. Always use `addTable()` (PptxGenJS) or a real `<a:tbl>` element (XML editing).**

Fake tables look fine visually but are impossible to edit in PowerPoint. Real tables are selectable, editable, resizable, and respond to theme changes.

```javascript
// CORRECT - real PowerPoint table, APEI brand
const rows = [
  [
    { text: "Initiative", options: { bold: true, color: "FFFFFF", fill: { color: "22314E" } } },
    { text: "Feasibility", options: { bold: true, color: "FFFFFF", fill: { color: "22314E" } } },
    { text: "Impact", options: { bold: true, color: "FFFFFF", fill: { color: "22314E" } } }
  ],
  ["Student Concierge", "Possible", "High"],
  ["Adaptive Learning", "Stretch", "High"],
  ["FA Automation", "Possible", "Medium"]
];

slide.addTable(rows, {
  x: 0.5, y: 1.2, w: 9, h: 2.5,
  colW: [3.5, 2.5, 3.0],
  border: { pt: 0.5, color: "E5E5E5" },
  rowH: 0.45,
  align: "left",
  valign: "middle",
  fontSize: 12,
  fontFace: "Open Sans"
});
```

**When to use a real table:**
- Comparisons (vendor A vs B vs C, initiative matrices)
- Grids with labeled rows and columns
- Schedules, roadmaps with columns
- Feature matrices, capability maps

**When a table is NOT needed:**
- Simple 2-item side-by-side layouts (use two text boxes)
- Stat callout grids (use shapes + text, no tabular relationship)
- Process flows (use connected shapes with arrows)

### Alternating Row Styles

When a table has more than 3 data rows, alternate row fills for readability. Use white and a tint of APEI Gray:

- Odd data rows: `#FFFFFF`
- Even data rows: `#F5F5F5` (light tint of APEI Gray)
- Header: `#22314E` with white text

### Column Widths

Always specify `colW` explicitly. `colW` array must sum to the table `w`.

```javascript
// Table w: 9 inches, 3 columns
colW: [3.5, 2.5, 3.0]  // sums to 9 ✅
```

---

## Text Inside Shapes: Embed, Don't Overlay

**Rule: When a shape contains a label or text, put the text inside the shape — don't overlay a separate text box.**

Overlaid text boxes look identical visually but create two independent objects. Moving the shape leaves the text behind. Embedding keeps them a single selectable unit.

```javascript
// CORRECT - text is part of the shape
slide.addText("Key Insight", {
  x: 0.5, y: 1.5, w: 3, h: 1,
  fill: { color: "22314E" },
  line: { color: "22314E", pt: 0 },
  shape: pres.shapes.RECTANGLE,
  color: "FFFFFF",
  bold: true,
  fontSize: 14,
  fontFace: "Open Sans",
  align: "center",
  valign: "middle"
});

// WRONG - shape and text are two separate objects
slide.addShape(pres.shapes.RECTANGLE, {
  x: 0.5, y: 1.5, w: 3, h: 1, fill: { color: "22314E" }
});
slide.addText("Key Insight", {
  x: 0.5, y: 1.5, w: 3, h: 1, color: "FFFFFF", bold: true
});
```

**This applies to:** labeled cards, callout boxes, icon + label pills, section dividers with titles, agenda number circles, any shape with a visible text label.

**Exception:** a shape used purely as a background or decorative element with no associated text may stand alone.

---

## General Layout Preferences

- **Fewer objects is better.** If two boxes can logically be one, make them one.
- **No fake visual elements.** If it looks like a table, it should be a table. If it looks like a bulleted list, it should be one text block with bullets, not separate text boxes.
- **Consistent padding inside cards.** Use `margin: [0.1, 0.15, 0.1, 0.15]` (top, right, bottom, left in inches) on text inside card-style shapes.
- **Preserve the template's signature elements.** The corner-mark triangles (top right), green underline accent, and section-title eyebrow appear on every content slide. Never remove them when editing.
- **Left-align body text.** Only titles, cover/closing slides, and large stat callouts get center alignment.
- **Minimum 0.5" margin from slide edges.** Don't crowd the corners.
- **Max 1 chart/diagram per slide.** If you have two, split across two slides.

---

## QA Checklist — Before Delivering to Iam / APEI

Run this before every handoff:

- [ ] Template used as the starting point (not rebuilt from scratch)
- [ ] All text uses **Open Sans** family (Light / Regular / Bold)
- [ ] Colors match the APEI palette exactly: Navy `#22314E`, Green `#CCDC00`, Gray `#E5E5E5`, Blue `#0072CF`
- [ ] No content that looks like a table is built with shapes + text boxes
- [ ] Text boxes with header + bullets are consolidated into single text blocks
- [ ] All tables use real table objects with explicit `colW`
- [ ] No text box is floating over a shape — text inside shapes is embedded
- [ ] All icons come from the template's icon library (or match its style exactly)
- [ ] Corner-mark triangle and green underline accent preserved on content slides
- [ ] Every slide title states an insight, not a topic (consulting-style action titles)
- [ ] No lorem ipsum, no "Slide Title," no placeholder names left behind:
      ```bash
      extract-text output.pptx | grep -iE "\bx{3,}\b|lorem|ipsum|\bTODO|\[insert|slide title|section title|full name|job title|presenter name"
      ```
- [ ] Object count per slide is as low as possible
- [ ] Visual QA pass done (PDF → pdftoppm → subagent inspection per the standard pptx SKILL.md)
- [ ] American English spelling throughout
- [ ] Footer / page numbers preserved

---

## Common Template Slide → Use-Case Mapping

Quick reference for mapping content to template slide layouts:

| Content type | Template slide(s) |
|--------------|------------------|
| Cover / title | Slide 1 |
| Agenda (2-column numbered) | Slide 2 |
| Section divider (light) | Slide 3, 6, 22, 34 |
| Section divider (dark) | Slide 35 (Questions), 36 (Thank You) |
| Text + single image (hero right) | Slide 4 |
| Three columns with images | Slide 5 |
| Three stat callouts | Slide 7 |
| Vision / Mission / Values (icon rows) | Slide 8 |
| Single executive bio (circle photo) | Slide 9 |
| Three executive bios | Slide 10 |
| Text + supporting image | Slides 11, 12, 13 |
| Two-stat callout | Slide 14 |
| Image + text, image left | Slide 15 |
| Text-heavy, multi-column | Slides 16, 17 |
| Three icon + subtitle rows | Slide 18 |
| Device mockup + annotations | Slides 19, 20, 21 |
| Bar + people infographic | Slide 23 |
| Venn/overlap diagram | Slide 24 |
| Donut + stat list | Slide 25 |
| Timeline / milestones | Slide 26 |
| Combo bar chart | Slide 27 |
| Column chart with annotations | Slide 28, 29 |
| Horizontal bar comparison | Slide 30 |
| Financial data table | Slide 31 |
| US map with stats | Slides 32, 33 |
| Closing — Questions | Slide 35 |
| Closing — Thank You | Slide 36 |
| Icon library (reference only) | Slides 37, 38 |

Prefer varied layouts across a deck — don't reuse slide 7 or 11 for every content slide.

---

## Summary of Non-Negotiables

1. **Start from the real APEI template.** Auto-fetch from GitHub by default; fall back to uploaded file; halt and ask if neither is available.
2. **Brand colors exact:** Navy `#22314E`, Green `#CCDC00`, Gray `#E5E5E5`, Blue `#0072CF`.
3. **Open Sans family only.** No Calibri, no Arial, no Helvetica on APEI decks.
4. **Template icons only.** No Lucide, no emoji, no invented icons.
5. **Consulting-grade writing.** Insight titles, MECE bullets, specific numbers, parallel structure.
6. **Real tables, consolidated text boxes, embedded shape text.** Editability matters.
7. **Preserve signature brand elements.** Corner triangles, green underline, section eyebrow.
8. **QA before delivery.** Visual pass + placeholder grep + brand color check.
