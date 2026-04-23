# APEI Slide Kit

Brand template, Claude skill, and reference assets for building board-ready PowerPoint decks in APEI's visual system.

## What's in here

| Path | What it is |
|------|------------|
| `templates/APEI_sanitized_ppttemplate.pptx` | Sanitized APEI brand template (38 layouts). Starting point for every deck. |
| `skills/APEI_PPT_SKILL.md` | Claude skill file. Drop this into Claude to get APEI-brand-aware slide generation. |
| `docs/brand-colors.md` | APEI color palette with hex, RGB, CMYK, and Pantone values. |
| `docs/slide-mapping.md` | Quick reference mapping content types to template slide layouts. |

## Quick start

### For Claude users

1. Upload `skills/APEI_PPT_SKILL.md` as a skill (or paste its contents into Claude's project instructions).
2. Ask Claude to build a deck. The skill will auto-fetch the template from this repo.

### For humans

1. Download `templates/apei_investor_presentation_template.pptx`.
2. Open in PowerPoint or Keynote.
3. Duplicate slides you want to use, replace the placeholder content, keep the corner-mark and green-underline brand elements intact.

## Raw URLs (for scripting)

```
https://raw.githubusercontent.com/Radius-Advisory/apei-slide-kit/main/templates/APEI_sanitized_ppttemplate.pptx
https://raw.githubusercontent.com/Radius-Advisory/apei-slide-kit/main/skills/APEI_PPT_SKILL.md
```

Replace `YOUR-USERNAME` with the actual GitHub account that owns this repo.

## Brand system at a glance

| Color | Hex | Use |
|-------|---------|-----|
| APEI Navy | `#22314E` | Primary brand color, title/closing slide backgrounds, headlines |
| APEI Green | `#CCDC00` | Sharp accent, corner marks, agenda numbers, underlines |
| APEI Gray | `#E5E5E5` | Neutral dividers, alternating table rows |
| APEI Blue | `#0072CF` | Secondary accent, charts, data viz |

Font family: **Open Sans** (Light, Regular, Bold).

## Notes

- The template has been sanitized: no executive photos, no real bios, no confidential content. It's safe to use as a public-distribution artifact.
- The full internal APEI template (with current executive bios and photos) lives on APEI's Brandfolder: <https://brandfolder.com/s/9thk4vrzm8jhfmrjqwgpf8b5>
- Updates and contributions welcome — open an issue or PR.

## License

Template layouts and brand design © American Public Education, Inc. The APEI logo and trademarks remain APEI's property. Skill files and documentation in this repo are provided as-is for use in creating APEI-aligned presentations.
