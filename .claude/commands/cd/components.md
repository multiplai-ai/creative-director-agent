---
name: cd/components
description: Build SVG component library from brand tokens. Generates native SVG backgrounds, textures, buttons, badges, dividers, card frames, icons, and logo lockups.
---

# /cd/components — SVG Component Library

Generate a library of native SVG components using exact values from tokens.json. Every component is Figma-importable as editable vector layers.

## Prerequisites

**HARD BLOCK:** `{entity-dir}/creative/brand-guide.md` AND `{entity-dir}/creative/tokens.json` must exist.
→ If missing: STOP. Say "Run `/cd/brand-guide` first."

## Inputs

1. **Entity name** (required)

## Workflow

### Step 1: Resolve & Load

1. Map entity name to `{entity-dir}`
2. Verify both prerequisites exist
3. Read `tokens.json` — extract all color, typography, texture, shape, button values

### Step 2: Present Component Inventory

| Category | Components | Count |
|----------|-----------|-------|
| **Backgrounds** | Light paper, dark paper, hero dark, accent subtle | 4 |
| **Textures** | Paper speckle, dot grid, micro grid, steno pad, fuzzy paper, mini dot | 6 |
| **Buttons** | Primary CTA, secondary CTA, ghost/tertiary | 3 |
| **Badges/Tags** | Category tag, status badge, label pill | 3 |
| **Dividers** | Thin line, section break, decorative rule | 3 |
| **Card Frames** | Content card, feature card, testimonial card | 3 |
| **Icons** | Arrow, check, close, external link, menu, plus, minus, search | 8 |
| **Logo Lockups** | Primary wordmark, dark variant, light variant, icon-only | 4 |

User confirms or adjusts the checklist.

### Step 3: Generate SVGs

For each component, generate **native SVG** following these rules:
- Use actual hex values from `tokens.json` — never approximate
- Use `<text>` elements with `font-family` from tokens (e.g., `font-family="YourFont, Georgia, serif"`)
- Use `<pattern>` for textures (dot grid = `<circle>` elements, paper = `<feTurbulence>`)
- Use `<filter>` with `<feTurbulence>` for noise textures
- Use `<rect>`, `<circle>`, `<line>`, `<path>` for shapes
- Every SVG has proper `viewBox`, `xmlns="http://www.w3.org/2000/svg"`, and descriptive comments
- Buttons include hover state as a separate `<g>` with `display="none"`
- Card frames include placeholder zones with dashed borders

**Naming convention:** `{category}-{variant}.svg`
Examples: `background-light-paper.svg`, `button-primary-cta.svg`, `texture-dot-grid.svg`

### Step 4: Save Components

Save to `{entity-dir}/creative/components/{category}/`:
- `backgrounds/`, `textures/`, `buttons/`, `badges/`, `dividers/`, `cards/`, `icons/`, `logo/`

### Step 5: Write Inventory

Generate `{entity-dir}/creative/component-inventory.md`:
- Table of every component: path, dimensions, category, usage notes
- Total count
- Last generated date

### Step 6: CHECKPOINT

User reviews generated components. Adjust if needed.

## Output

- `{entity-dir}/creative/components/` — SVG files organized by category
- `{entity-dir}/creative/component-inventory.md` — manifest

## Next Step

→ Run `/cd/templates` to build platform-specific templates.
