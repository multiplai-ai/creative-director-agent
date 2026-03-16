---
name: cd/sop
description: Operating procedure for the Creative Director suite. Explains the 5-stage foundation, production workflow, storage architecture, and tool integration.
---

# The Creative Director Suite: How It Works

A standard operating procedure for running the creative foundation and production workflow from visual research to campaign assets.

---

## What Is the Creative Director Suite?

The CD suite is a sequence of skills that take a brand from "we need a visual identity" to "here are production-ready assets across every channel."

Each skill builds on the one before it. The output of one becomes the required input for the next. Prerequisites are enforced — you can't skip stages.

**Core principle: Repo is hub, tools are adapters.** All brand foundations live in git as design tokens + SVGs + reference code. Downstream tools (Gamma, Pencil, Figma, Canva) consume from the repo via translation scripts. When tools change, update the script, not the library.

---

## The Five Foundation Stages (Run Once Per Brand)

```
/cd/research    → visual-research.md + references/ (harvested code + SVGs)
      ↓
/cd/mood        → mood-direction.md (feel, energy, prompts — NO colors/fonts)
      ↓
/cd/brand-guide → brand-guide.md + tokens.json (colors, fonts, textures, rules, locked AI prompts)
      ↓
/cd/components  → components/ (native SVGs) + component-inventory.md
      ↓
/cd/templates   → templates/ (native SVGs) + template-inventory.md
```

### Production (Run Per Campaign)

```
/cd/produce     → Routes by asset type, loads all context, produces on-brand output
```

### Supporting

```
/cd/status      → Show foundation progress for any entity
/cd/sop         → This document
```

---

## Storage Architecture

All creative artifacts live in `{entity-dir}/creative/`. The structure:

```
{entity-dir}/creative/
├── visual-research.md          ← Stage 1 output
├── mood-direction.md           ← Stage 2 output
├── brand-guide.md              ← Stage 3 output (human-readable)
├── tokens.json                 ← Stage 3 output (W3C design tokens, machine-readable)
├── component-inventory.md      ← Stage 4 manifest
├── template-inventory.md       ← Stage 5 manifest
├── references/                 ← Harvested code from research
│   ├── competitors/
│   ├── aspirational/
│   └── screenshots/
├── components/                 ← Native SVG components
│   ├── backgrounds/
│   ├── textures/
│   ├── buttons/
│   ├── badges/
│   ├── dividers/
│   ├── cards/
│   ├── icons/
│   └── logo/
├── templates/                  ← Platform SVG templates
│   ├── linkedin/
│   ├── instagram/
│   ├── twitter/
│   ├── ads/
│   ├── email/
│   ├── web/
│   └── og/
└── generated/                  ← Auto-generated from tokens.json
    ├── variables.css
    ├── tailwind.config.js
    ├── gamma-prompts.md
    ├── pencil-variables.json
    └── figma-tokens.json
```

### Three Storage Layers

| Layer | Format | Purpose | Example |
|-------|--------|---------|---------|
| **Design Tokens** | JSON (W3C DTCG) | Machine-readable values — colors, fonts, spacing | `tokens.json` |
| **Vector Assets** | Native SVG | Figma-editable components — backgrounds, textures, buttons | `components/backgrounds/light-paper-01.svg` |
| **Reference Code** | HTML + Tailwind | Harvested "vibe code" from inspiration sites | `references/aspirational/stripe-hero.html` |

---

## How Upstream Skills Feed Downstream

| Upstream Skill | Produces | Consumed By |
|----------------|----------|-------------|
| Discovery / Strategy | `discovery-intake.md` | `/cd/research` (soft — extracts positioning, ICP, competitors) |
| Positioning | `positioning-strategy.md` | `/cd/research`, `/cd/mood` (extracts differentiation) |
| Brand Strategy | `brand-strategy.md` | `/cd/mood`, `/cd/brand-guide` (extracts voice, personality) |
| `/cd/research` | `visual-research.md` + `references/` | `/cd/mood` (hard block) |
| `/cd/mood` | `mood-direction.md` | `/cd/brand-guide` (hard block) |
| `/cd/brand-guide` | `brand-guide.md` + `tokens.json` | `/cd/components` (hard block), `/cd/produce` |
| `/cd/components` | `components/` + `component-inventory.md` | `/cd/templates` (hard block), `/cd/produce` |
| `/cd/templates` | `templates/` + `template-inventory.md` | `/cd/produce` |

**Hard block** = skill refuses to run without the upstream artifact.
**Soft** = skill extracts what it can, proceeds with explicit assumptions if missing.

---

## Translation Scripts

These read `tokens.json` and output tool-specific configs:

| Script | Input | Output |
|--------|-------|--------|
| `tools/tokens_to_css.py` | `tokens.json` | `generated/variables.css` |
| `tools/tokens_to_tailwind.py` | `tokens.json` | `generated/tailwind.config.js` |
| `tools/tokens_to_gamma.py` | `tokens.json` | `generated/gamma-prompts.md` |
| `tools/tokens_to_pencil.py` | `tokens.json` | `generated/pencil-variables.json` |
| `tools/tokens_to_figma.py` | `tokens.json` | `generated/figma-tokens.json` |

---

## Tool Roles

| Tool | Role | When to Use |
|------|------|-------------|
| **Claude Code** | Orchestrator + SVG generator | Always — runs skills, generates native SVG, manages tokens |
| **Gamma** | AI image generator | Backgrounds, textures, visual content (locked brand prompts) |
| **Figma** | Precision editor | Final brand asset refinement, component library editing |
| **Pencil** | UI-to-code generator | Web/app UI design that becomes React/Tailwind code |
| **Canva** | Speed compositor | Quick social graphics, client collaboration |

---

## Key Design Principles

1. **Repo is hub, tools are adapters.** When tools change, update the translation script, not the library.
2. **Upstream determines downstream.** Strategy → creative foundation → production. No skipping.
3. **Capture code, not screenshots.** Harvested references stored as HTML/Tailwind/SVG, not PNGs.
4. **Design tokens are the source of truth.** `tokens.json` feeds everything.
5. **Explicit prerequisites with hard blocks.** Missing upstream artifact = stop and redirect.
6. **Checkpoints at every stage.** User approves before proceeding.
7. **Locked prompts stored as artifacts.** AI image prompts generated once, stored permanently.
8. **Campaign-aware production.** Every asset connects to a campaign or content calendar item.
9. **Native SVG for all components and templates.** Never HTML-to-SVG conversion.

---

## Quick Reference

- **Check foundation progress:** `/cd/status`
- **Start a new brand:** `/cd/research` → follow the chain
- **Produce assets:** `/cd/produce` (requires complete foundation)
- **All artifacts save to:** `{entity-dir}/creative/`
