---
name: cd/brand-guide
description: Brand guidelines + design tokens. 7 sub-stages with checkpoints. Produces brand-guide.md (human-readable) + tokens.json (W3C DTCG machine-readable). Runs translation scripts for CSS, Tailwind, Gamma, Pencil, Figma.
---

# /cd/brand-guide — Brand Guidelines + Design Tokens

Lock in every visual decision — colors, typography, textures, buttons, imagery, AI prompts — and compile into both a human-readable brand guide and machine-readable W3C design tokens.

## Prerequisites

**HARD BLOCK:** `{entity-dir}/creative/mood-direction.md` must exist.
→ If missing: STOP. Say "Run `/cd/mood` first."

**Soft (load if available):**
- `{entity-dir}/creative/visual-research.md` + `references/`
- `{entity-dir}/strategy/brand-strategy.md`

## Inputs

1. **Entity name** (required)

## Workflow

### Step 1: Resolve & Load

1. Map entity name to `{entity-dir}`
2. Verify `mood-direction.md` exists
3. Read: mood-direction.md, visual-research.md, references/, brand-strategy.md (if exists)

### Sub-Stage A: Colors

1. Generate 2-3 palette options, each with:
   - **Primary palette** (4-5 colors): background, text, text-secondary, darkest, surface
   - **Extended palette** (5-8 colors): accent, callout, border, hover, status colors
   - **Contrast ratios** (WCAG AA minimum, AAA preferred)
   - **Color rules**: what's allowed, what's not, opacity, gradient policy
2. Present with hex codes, RGB, usage rules, and contrast checks
3. **CHECKPOINT: User selects palette**

### Sub-Stage B: Typography

1. Generate 2-3 pairings, each with:
   - Display/headline font (weight, size range, source)
   - Body font (weight, size range, source)
   - Mono/code font (weight, size range, source)
   - Full type scale: Display, H1, H2, H3, H4, Body, Body Small, Caption, Label, Code
   - Line heights, letter spacing, max line length
   - Font sources (Google Fonts URL, license, Canva/Figma availability, fallback stack)
2. **CHECKPOINT: User selects typography**

### Sub-Stage C: Textures & Surfaces

1. Define surface treatments from mood direction:
   - List each texture: name, description, opacity range, implementation (SVG `<pattern>`, `<filter>`, CSS)
   - Surface depth policy: flat vs layered, shadow approach
   - Rule: "every surface textured" vs "selective texture"
   - Layering order: base color → texture → content
2. User reviews

### Sub-Stage D: Buttons & Interactive Elements

1. Define for Primary, Secondary, Ghost CTAs:
   - Background, text color, font, font-style, font-weight, font-size
   - Padding, border-radius, border
   - Hover, active, focus, disabled states
2. Shape language: border-radius, shadow, border-width, border-color

### Sub-Stage E: Imagery Rules

1. Subjects, composition, color treatment, avoid list
2. Icon style: stroke weight, caps, sizes, color rules, icon set
3. Logo usage: placement, minimum size, clear space, dark/light variants

### Sub-Stage F: Voice-Visual Alignment

1. Map each brand voice trait to visual execution
2. Channel-specific overrides: LinkedIn, Instagram, Twitter, Email, Website, Ads, Video, PDF

### Sub-Stage G: AI Prompt Generation

1. Generate **locked prompts** (stored permanently, loaded by `/cd/produce`):
   - **Gamma Image Style** — positive + negative keywords, visual rules
   - **Midjourney template** — style keywords, parameters
   - **General AI image prompt** — tool-agnostic
2. **CHECKPOINT: User locks AI prompts**

### Final Compilation

1. Write `{entity-dir}/creative/brand-guide.md` — human-readable, all sections
2. Write `{entity-dir}/creative/tokens.json` — W3C DTCG format with all values:
   - `brand` (name, tagline, personality)
   - `color` (primary, accent, status — each token has `$value`, `$type`, `$description`)
   - `typography` (display, h1-h4, body, bodySmall, caption, label, code — each with fontFamily, fontWeight, fontSize, lineHeight, letterSpacing)
   - `spacing` (base, scale array)
   - `shape` (borderRadius, cardRadius, borderWidth, borderColor, shadow)
   - `button` (primary, secondary, ghost — full specs)
   - `texture` (each texture with opacity, description, implementation)
   - `imagery` (style, subjects, avoid, positivePrompt, negativePrompt, gammaImageStyle)
   - `icon` (style, strokeWeight, caps, sizes, color)
   - `logo` (type, minimumWidth, clearSpace, placement)
   - `layout` (maxLineLength, contentSafeZone, margins, sectionPadding)

3. Run translation scripts:
```bash
python3 tools/tokens_to_css.py {entity-dir}/creative/tokens.json -o {entity-dir}/creative/generated/variables.css
python3 tools/tokens_to_tailwind.py {entity-dir}/creative/tokens.json -o {entity-dir}/creative/generated/tailwind.config.js
python3 tools/tokens_to_gamma.py {entity-dir}/creative/tokens.json -o {entity-dir}/creative/generated/gamma-prompts.md
python3 tools/tokens_to_figma.py {entity-dir}/creative/tokens.json -o {entity-dir}/creative/generated/figma-tokens.json
python3 tools/tokens_to_pencil.py {entity-dir}/creative/tokens.json -o {entity-dir}/creative/generated/pencil-variables.json
```

## Output

- `{entity-dir}/creative/brand-guide.md`
- `{entity-dir}/creative/tokens.json`
- `{entity-dir}/creative/generated/` — CSS, Tailwind, Gamma, Figma, Pencil configs

## Anti-Patterns

- Never skip a sub-stage checkpoint
- Never use approximate colors — exact hex codes only
- Never generate AI prompts that include text/copy — visual prompts only
- Anti-patterns section in brand-guide.md is mandatory

## Next Step

→ Run `/cd/components` to build the SVG component library.
