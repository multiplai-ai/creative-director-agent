# CLAUDE.md — Creative Director Agent

## What This Is

The Creative Director Agent is an AI-powered brand identity and creative production system. It works as slash commands in Claude Code, Cursor, or any environment that supports `.claude/commands/`.

## The 5-Stage Foundation Chain

Run once per brand, in order:

```
/cd/research    → Visual research + competitive analysis
/cd/mood        → Mood direction (feel, energy — NO colors/fonts)
/cd/brand-guide → Brand guide + tokens.json (colors, fonts, textures, rules)
/cd/components  → Native SVG component library
/cd/templates   → Platform-specific SVG templates
```

Each stage has hard prerequisites — the suite will stop and redirect if upstream artifacts are missing.

## Production

After the foundation is complete:

```
/cd/produce     → Campaign-aware asset production (routes by asset type)
```

## Entity Structure

Brands live in `entities/{brand-name}/` with two subdirectories:

- `strategy/` — Upstream strategy docs (discovery, positioning, brand strategy, ICP, content strategy). These are soft dependencies — the CD suite extracts what's available and proceeds with assumptions if missing.
- `creative/` — All creative artifacts managed by the CD suite. Includes `tokens.json` (W3C DTCG design tokens), SVG components, templates, and generated tool configs.

## Translation Tools

Python scripts in `tools/` convert `tokens.json` to platform-specific formats:

- `tokens_to_css.py` → CSS custom properties
- `tokens_to_tailwind.py` → Tailwind CSS config
- `tokens_to_gamma.py` → Gamma AI image prompts
- `tokens_to_figma.py` → Figma Tokens Studio JSON
- `tokens_to_pencil.py` → Pencil variables JSON

These run automatically at the end of `/cd/brand-guide`.

## Key Principles

1. **Repo is hub, tools are adapters.** All brand foundations live in git. Downstream tools consume via translation scripts.
2. **Design tokens are the source of truth.** `tokens.json` feeds everything.
3. **Native SVG always.** Real `<text>`, `<rect>`, `<pattern>`, `<filter>` elements — never HTML-to-SVG conversion.
4. **Upstream determines downstream.** Strategy → creative foundation → production. No skipping.
5. **Checkpoints at every stage.** User approves before proceeding.

## Getting Started

```
/cd/research {brand-name}
```

Follow the chain from there. Run `/cd/status` at any time to check progress. Run `/cd/sop` for the full operating procedure.
