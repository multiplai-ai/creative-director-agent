---
name: cd/templates
description: Build platform-specific SVG templates with placeholder zones. Uses brand components and tokens for consistent cross-platform production.
---

# /cd/templates — Platform Templates

Generate SVG templates for every platform with branded backgrounds, placeholder zones, and pre-placed logo lockups.

## Prerequisites

**HARD BLOCK:** `{entity-dir}/creative/component-inventory.md` must exist.
→ If missing: STOP. Say "Run `/cd/components` first."

**Also loads:**
- `{entity-dir}/creative/brand-guide.md`
- `{entity-dir}/creative/tokens.json`
- `{entity-dir}/creative/visual-research.md` (soft — for channel priorities)
- `{entity-dir}/strategy/content-strategy.md` (soft — for primary channels)

## Inputs

1. **Entity name** (required)

## Workflow

### Step 1: Resolve & Load

1. Map entity name to `{entity-dir}`
2. Verify `component-inventory.md` exists
3. Read tokens.json, brand-guide.md, component inventory

### Step 2: Present Template Inventory

| Platform | Templates | Dimensions |
|----------|-----------|------------|
| **LinkedIn** | Single image post, carousel slide, banner | 1200x1200, 1080x1350, 1584x396 |
| **Instagram** | Square post, story/reel cover, carousel slide | 1080x1080, 1080x1920, 1080x1350 |
| **Twitter/X** | Post image, header | 1200x675, 1500x500 |
| **Ads** | Static landscape, static square, static story | 1200x628, 1080x1080, 1080x1920 |
| **Email** | Header banner, inline graphic | 600x200, 600x400 |
| **Web** | Hero section, OG image, feature card | 1440x800, 1200x630, 400x300 |

User confirms or adjusts.

### Step 3: Generate Templates

For each template, generate native SVG with:
- Brand-standard background texture (from components)
- Placeholder zones with descriptive `id` attributes: `headline-zone`, `subhead-zone`, `body-zone`, `image-zone`, `cta-zone`, `logo-zone`
- Placeholder zones shown as dashed-border rectangles with label text
- Brand colors and typography specs as XML comments
- Logo lockup pre-placed in standard position (per brand-guide)
- Content safe zone (80% width) marked

**Naming convention:** `{platform}-{variant}.svg`
Examples: `linkedin-single-post.svg`, `ads-static-landscape.svg`

### Step 4: Save Templates

Save to `{entity-dir}/creative/templates/{platform}/`:
- `linkedin/`, `instagram/`, `twitter/`, `ads/`, `email/`, `web/`

### Step 5: Write Inventory

Generate `{entity-dir}/creative/template-inventory.md`:
- Table: path, platform, dimensions, placeholder zones, usage notes
- Total count, last generated date

### Step 6: CHECKPOINT

User reviews templates. Adjust if needed.

## Output

- `{entity-dir}/creative/templates/` — SVG files by platform
- `{entity-dir}/creative/template-inventory.md` — manifest

## Foundation Complete

All 5 stages done. Run `/cd/status` to verify, or `/cd/produce` to create assets.
