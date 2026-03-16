---
name: cd/status
description: Check Creative Director foundation progress for any entity. Shows which stages are complete, what's missing, and what to run next.
---

# /cd/status — Creative Director Foundation Progress

Check which creative foundation stages are complete for an entity and what to run next.

## Inputs

User provides ONE of:
1. **Entity name** — "Acme Corp", "ExampleClient", etc.
2. **No input** — Check all entities with creative directories

## Workflow

### Step 1: Resolve Entity Directory

Map entity name to directory:
- Check `entities/{name}/` (lowercase, slugified)
- If not found, ask the user for the correct path

Set `{entity-dir}` to the resolved path.

### Step 2: Check Foundation Artifacts

For `{entity-dir}/creative/`, check existence of each artifact:

| Stage | Skill | Artifact | Status Check |
|-------|-------|----------|-------------|
| 1. Research | `/cd/research` | `visual-research.md` | File exists and has content |
| 1b. References | `/cd/research` | `references/` directory | Directory exists with >=1 file |
| 2. Mood | `/cd/mood` | `mood-direction.md` | File exists and has content |
| 3. Brand Guide | `/cd/brand-guide` | `brand-guide.md` | File exists and has content |
| 3b. Tokens | `/cd/brand-guide` | `tokens.json` | File exists and is valid JSON |
| 3c. Generated | `/cd/brand-guide` | `generated/` directory | Contains >=1 generated file |
| 4. Components | `/cd/components` | `component-inventory.md` | File exists and has content |
| 4b. SVGs | `/cd/components` | `components/` directory | Directory exists with >=1 SVG |
| 5. Templates | `/cd/templates` | `template-inventory.md` | File exists and has content |
| 5b. SVGs | `/cd/templates` | `templates/` directory | Directory exists with >=1 SVG |

### Step 3: Check Upstream Strategy Context

Also check for strategy artifacts that feed the creative foundation:

| Strategy Artifact | Location | Used By |
|-------------------|----------|---------|
| Discovery | `{entity-dir}/strategy/discovery-intake.md` | `/cd/research` (soft) |
| Positioning | `{entity-dir}/strategy/positioning-strategy.md` | `/cd/research` (soft) |
| Brand Strategy | `{entity-dir}/strategy/brand-strategy.md` | `/cd/mood`, `/cd/brand-guide` (soft) |
| ICP & Personas | `{entity-dir}/strategy/icp-personas.md` | `/cd/research` (soft) |
| Content Strategy | `{entity-dir}/strategy/content-strategy.md` | `/cd/templates` (soft) |

### Step 4: Output Status Report

Format:

```
## Creative Director Status: {Entity Name}
Entity directory: {entity-dir}

### Foundation Progress
| # | Stage | Status | Artifact |
|---|-------|--------|----------|
| 1 | Research | Complete / Missing | visual-research.md |
| 2 | Mood | Complete / Missing | mood-direction.md |
| 3 | Brand Guide | Complete / Missing | brand-guide.md + tokens.json |
| 4 | Components | Complete / Missing | components/ + inventory |
| 5 | Templates | Complete / Missing | templates/ + inventory |

### Strategy Context
| Artifact | Status |
|----------|--------|
| Discovery | Available / Missing |
| Positioning | Available / Missing |
| Brand Strategy | Available / Missing |
| ICP & Personas | Available / Missing |
| Content Strategy | Available / Missing |

### Production Ready?
{Yes — all 5 stages complete, /cd/produce available}
{No — next step: run /cd/{next-stage}}

### Next Action
→ Run `/cd/{next-incomplete-stage}` to continue the foundation.
```

### Step 5: If Checking All Entities

If no entity specified, scan all entity directories for `creative/` subdirectories and output a summary table:

```
## Creative Director Status: All Entities

| Entity | Dir | Research | Mood | Brand Guide | Components | Templates | Production Ready |
|--------|-----|----------|------|-------------|------------|-----------|-----------------|
| Acme Corp | entities/acme-corp/ | Complete | Complete | Complete | Missing | Missing | No |
```
