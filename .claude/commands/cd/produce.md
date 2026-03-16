---
name: cd/produce
description: Campaign-aware production routing. Loads brand tokens, components, and templates to produce on-brand assets. Routes by asset type — static visuals, content visuals, or multi-channel campaigns.
---

# /cd/produce — Production Routing

Produce on-brand assets using the complete creative foundation. Routes by asset type, loads all context automatically, enforces brand compliance.

## Prerequisites

**HARD BLOCK:** All three must exist:
- `{entity-dir}/creative/brand-guide.md`
- `{entity-dir}/creative/components/` (with SVGs)
- `{entity-dir}/creative/templates/` (with SVGs)

→ If missing: STOP. Say "Run `/cd/status` to check what's missing."

**Auto-loaded every production run:**
- `{entity-dir}/creative/tokens.json`
- `{entity-dir}/creative/generated/gamma-prompts.md` (locked AI prompts)
- `{entity-dir}/creative/component-inventory.md`
- `{entity-dir}/creative/template-inventory.md`
- Strategy docs: positioning, brand strategy, ICP (soft — for campaign context)

## Inputs

1. **Entity name** (required)
2. **What to produce** — user describes the asset(s)

## Workflow

### Step 1: Campaign Context

Ask user:
- What campaign or content calendar item? (or one-off?)
- Goal: awareness, conversion, engagement?
- Channels: LinkedIn, Instagram, ads, email, web?

### Step 2: Asset Scope

- **Single asset** — one visual, one channel
- **Multi-format batch** — same concept across channels
- **Full campaign set** — multiple concepts across channels

### Step 3: Route to Sub-Workflow

---

#### Route A: Static Visual (ads, social graphics)

1. **Copy generation** — minimum 30 headline + subheadline pairs, organized by messaging wedge
2. User selects 5-10 for current batch
3. **Shot list** — 30-50 specific shot descriptions:
   - Each: composition, subject, mood, color treatment, texture
   - Uses locked Gamma prompts from `generated/gamma-prompts.md`
4. User selects/ranks shots
5. **Gamma image generation** — TWO prompts per call:
   - **Prompt A (Image Style):** Locked prompt from `generated/gamma-prompts.md`
   - **Prompt B (Input Text):** Business context + brand guidelines + global rules + specific shot description
6. **Assembly** — combine images + copy overlays using templates
7. **QC checklist** (see below)

#### Route B: Content Visual (infographics, diagrams, frameworks)

1. Analyze content → determine visual type (framework, process, matrix, comparison, etc.)
2. Extract elements: title, sections, labels, relationships
3. Load brand tokens → generate native SVG or HTML with exact brand values
4. **QC checklist**

#### Route C: Multi-Channel Campaign

1. Create unified creative brief
2. Per-channel production (adapt copy to channel voice from brand-guide overrides)
3. Select template per channel, produce via Route A or B
4. **Batch QC** — cross-channel consistency

---

### Step 4: Quality Control (All Routes)

Mandatory checklist:
- [ ] All colors match tokens.json exactly (no approximations)
- [ ] Typography matches type scale (correct fonts, weights, sizes)
- [ ] Textures applied to all surfaces (no flat fills)
- [ ] Logo placed correctly per brand-guide
- [ ] Contrast ratios meet WCAG AA
- [ ] No anti-pattern violations
- [ ] Asset dimensions match platform specs
- [ ] CTA uses brand button style
- [ ] Copy is direct, action-oriented, no banned phrases

## Non-Negotiable Production Rules

1. **Always load entity context first** — never produce from brand profile alone
2. **Minimum 30 ad copies per batch** (headline + subheadline pairs)
3. **Two prompts per Gamma call** — Image Style (locked) + Input Text (per shot)
4. **Gamma is a visual factory** — NEVER send copy for text generation
5. **30-50 shot descriptions per shot list** (separate analysis step)
6. **Copy is TEXT OVERLAY** — never generated inside images
7. **Use exact token values** — no "close enough" colors or fonts
8. **Native SVG always** — real `<text>`, `<rect>`, `<pattern>`, `<filter>` elements

## Output

- Assets saved to `{entity-dir}/creative/output/{campaign-name}/`
- Production log: what was generated, tools used, prompts used

## After Production

Distribute via your content distribution workflow or manually upload to platforms.
