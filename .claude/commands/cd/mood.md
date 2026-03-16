---
name: cd/mood
description: Mood board & visual direction. Generates 2-3 mood concepts from research, defines feel/energy/vibe (NOT colors or fonts), and produces AI image prompts.
---

# /cd/mood — Mood Board & Visual Direction

Create the visual direction for a brand. Defines feel, energy, texture approach, and composition — but NOT specific colors or fonts (those come in `/cd/brand-guide`).

## Prerequisites

**HARD BLOCK:** `{entity-dir}/creative/visual-research.md` must exist.
→ If missing: STOP. Say "Run `/cd/research` first to build the visual research foundation."

**Soft (load if available):**
- `{entity-dir}/strategy/brand-strategy.md` — voice and personality traits

## Inputs

1. **Entity name** (required) — e.g., "Acme Corp", "ExampleClient"

## Workflow

### Step 1: Resolve Entity Directory

Map entity name to directory path:
- Check `entities/{name}/` (lowercase, slugified)
- If not found, ask the user for the correct path
- Set `{entity-dir}` to the resolved path
- Verify `{entity-dir}/creative/visual-research.md` exists — if not, STOP

### Step 2: Load Context

Read in order:
1. `{entity-dir}/creative/visual-research.md` — competitive analysis, pattern findings
2. `{entity-dir}/creative/references/` — scan harvested code files for structural patterns
3. `{entity-dir}/strategy/brand-strategy.md` (if exists) — extract personality traits, voice descriptors

### Step 3: Extract Brand Personality

From brand strategy (or ask user if missing):
- What 3-5 personality traits need visual expression?
- What mood should someone feel when they encounter this brand?
- What's the brand's "energy" — calm authority? bold disruption? warm expertise?

### Step 4: Generate 2-3 Mood Concepts

Each concept includes:

| Dimension | Description |
|-----------|-------------|
| **Mood Name** | 2-3 words capturing the essence |
| **Feel / Energy** | 3-5 descriptors (warm, technical, editorial, etc.) |
| **Visual Metaphor** | What real-world thing does this look/feel like? |
| **Texture Direction** | Smooth vs rough, flat vs layered, matte vs glossy |
| **Composition Approach** | Dense vs spacious, grid vs organic, symmetrical vs dynamic |
| **Imagery Direction** | Photographic vs illustrated vs diagrammatic vs abstract |
| **Emotional Temperature** | Warm vs cool, approachable vs authoritative |
| **"Inspired by" References** | Which harvested references from research inform this |
| **Differentiation** | What makes this different from competitors (from research) |

**CRITICAL RULE: No colors. No fonts.** Those decisions come in `/cd/brand-guide`. Mood is about FEEL, not specifics. If you catch yourself writing hex codes or font names, stop.

### Step 5: CHECKPOINT — Present Concepts

Present all 2-3 concepts side by side. Ask user:
- "Which concept resonates? Or should I hybridize elements from multiple?"
- User selects one or describes a hybrid

### Step 6: Expand Selected Direction

Build out the chosen concept into a full mood direction:

1. **Detailed feel description** — 2-3 paragraphs capturing the visual experience
2. **Composition rules** — alignment, whitespace, hierarchy, grid approach
3. **Texture approach** — how textured vs clean, layering order, depth
4. **Imagery direction** — subjects, treatments, what to avoid
5. **"Is / Is Not" table** — clear boundaries

| This IS | This IS NOT |
|---------|-------------|
| (from the mood) | (opposite) |

6. **AI image prompts** — Tool-agnostic positive and negative keyword lists that capture this mood. These are mood-level prompts, not brand-specific — they'll be refined in `/cd/brand-guide`.
7. **Connection to strategy** — How this mood maps to positioning anchor, brand personality, and competitive differentiation

### Step 7: CHECKPOINT — Mood Direction Approved

Present the full mood direction. User confirms or requests adjustments.

## Output

Save to `{entity-dir}/creative/mood-direction.md` with sections:
- Mood Direction Name
- Feel & Energy
- Visual Metaphor
- Composition Rules
- Texture Approach
- Imagery Direction
- What This Is / What This Is Not
- AI Prompt Keywords (Positive)
- AI Prompt Keywords (Negative)
- Connection to Strategy
- Reference Mapping

## Anti-Patterns

- Never include specific colors (hex codes, color names beyond mood descriptors like "warm" or "cool")
- Never include specific fonts
- Never copy a reference directly — "inspired by" means differentiated adaptation
- Never skip the "Is / Is Not" table — it's the most useful boundary-setting tool
- Never skip the strategy connection — mood must tie back to business positioning

## Next Step

→ Run `/cd/brand-guide` to lock in colors, typography, and design tokens.
