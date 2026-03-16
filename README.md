# Creative Director Agent

An AI-powered brand identity and creative production system. It takes you from visual research through mood direction, brand guidelines with design tokens, an SVG component library, platform templates, and finally to production-ready campaign assets — all orchestrated through slash commands in Claude Code, Cursor, or any AI coding agent that supports `.claude/commands/`.

The system stores everything as code: W3C design tokens in JSON, native SVG components, and reference HTML/Tailwind. Downstream tools (Gamma, Figma, Pencil, Canva) consume from the repo via translation scripts. When tools change, you update the script — not the library.

## Requirements

- **Claude Code** or **Cursor** with Claude (any environment supporting `.claude/commands/`)
- **Python 3.10+** (for translation tools)
- **WebFetch capability** (for competitive research in Stage 1)

## Quick Start

```bash
# Clone the repo
git clone https://github.com/multiplai-ai/creative-director-agent.git
cd creative-director-agent

# Create your brand directory
mkdir -p entities/your-brand/strategy entities/your-brand/creative

# Start the foundation chain
# In Claude Code or Cursor:
/cd/research your-brand
```

Follow the chain: research → mood → brand-guide → components → templates → produce.

## The 5-Stage Foundation Pipeline

Each stage builds on the previous one. Prerequisites are enforced — you cannot skip stages.

| Stage | Command | Output | Description |
|-------|---------|--------|-------------|
| 1 | `/cd/research` | `visual-research.md` + `references/` | Competitive analysis, visual pattern synthesis, harvested code |
| 2 | `/cd/mood` | `mood-direction.md` | Feel, energy, texture direction — NO colors or fonts yet |
| 3 | `/cd/brand-guide` | `brand-guide.md` + `tokens.json` | Colors, typography, textures, buttons, imagery rules, locked AI prompts |
| 4 | `/cd/components` | `components/` + inventory | Native SVG library: backgrounds, textures, buttons, badges, cards, icons |
| 5 | `/cd/templates` | `templates/` + inventory | Platform-specific SVG templates with placeholder zones |

### Production

Once the foundation is complete, run `/cd/produce` for campaign-aware asset production. It routes by asset type (static visuals, content visuals, multi-channel campaigns), loads all brand context automatically, and enforces brand compliance.

### Supporting Commands

- `/cd/status` — Check foundation progress for any entity
- `/cd/sop` — Full operating procedure documentation

## Translation Tools

The `tools/` directory contains Python scripts that convert `tokens.json` into tool-specific formats:

| Script | Output | Use Case |
|--------|--------|----------|
| `tokens_to_css.py` | `variables.css` | CSS custom properties for web projects |
| `tokens_to_tailwind.py` | `tailwind.config.js` | Tailwind CSS theme extension |
| `tokens_to_gamma.py` | `gamma-prompts.md` | Structured prompts for Gamma AI image generation |
| `tokens_to_figma.py` | `figma-tokens.json` | Figma Tokens Studio plugin format |
| `tokens_to_pencil.py` | `pencil-variables.json` | Pencil UI-to-code variable format |

Usage:

```bash
python3 tools/tokens_to_css.py entities/your-brand/creative/tokens.json \
  -o entities/your-brand/creative/generated/variables.css
```

All translation scripts are run automatically at the end of `/cd/brand-guide`.

## Entity Directory Structure

Each brand lives in its own directory under `entities/`:

```
entities/your-brand/
├── strategy/                    # Upstream strategy docs (optional, soft dependencies)
│   ├── discovery-intake.md
│   ├── positioning-strategy.md
│   ├── brand-strategy.md
│   ├── icp-personas.md
│   └── content-strategy.md
└── creative/                    # All creative artifacts (managed by CD suite)
    ├── visual-research.md       # Stage 1
    ├── mood-direction.md        # Stage 2
    ├── brand-guide.md           # Stage 3 (human-readable)
    ├── tokens.json              # Stage 3 (machine-readable, W3C DTCG)
    ├── references/              # Harvested code from research
    ├── components/              # Native SVG component library
    ├── templates/               # Platform SVG templates
    └── generated/               # Auto-generated tool configs
```

Strategy documents are optional soft dependencies — the CD suite extracts what it can from them and proceeds with explicit assumptions if they're missing.

## Upstream Strategy

The Creative Director Agent focuses on visual identity and asset production. For the upstream strategy suite (discovery, positioning, brand strategy, ICP personas, content strategy), see the [CMO Agent](https://github.com/multiplai-ai/cmo-agent).

## License

MIT

## Credit

Built by [MultiplAI Growth Systems](https://multiplai.co)
