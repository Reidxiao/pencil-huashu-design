# pencil-huashu-design

> AI Agent 设计技能：一句话出高保真 UI 设计稿，自带反 AI-slop 系统

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A production-ready design skill for AI coding agents that turns a single prompt into professional UI design in [Pencil](https://pencil.evolus.vn/). Built with the anti-AI-slop methodology from [huashu-design](https://github.com/alchaincyf/huashu-design).

**Supported Agents**: [Claude Code](https://claude.ai/code) / [Cursor](https://cursor.sh/) / [Windsurf](https://codeium.com/windsurf) / Any agent that can read files and call MCP tools

---

## What It Does

<!-- TODO: Add before/after demo GIF here -->
<!-- Expected: Left side = typical AI-generated slop, Right side = this skill's output -->

**Before** (typical AI-generated design):
> Purple gradient background, Inter Bold 32px title, 3 equal cards with left-border accent, emoji icons

**After** (this skill's output):
> Clean warm-white background, Newsreader serif title with clear hierarchy, asymmetric layout with intentional rhythm, Lucide icon system

The skill forces the Agent through a structured workflow that prevents common AI design mistakes and produces genuinely professional results.

---

## Quick Start

### 1. Prerequisites

- [Pencil](https://pencil.evolus.vn/) installed (free, open-source)
- Pencil MCP server configured in your agent
  - **Claude Code**: Install the Pencil MCP server via your MCP settings (see [Pencil VS Code extension](https://marketplace.visualstudio.com/items?itemName=pencil.pencil-vscode) or standalone MCP server)
  - **Cursor**: Add Pencil MCP to your `.cursor/mcp.json`
  - Ensure Pencil is open and MCP is connected before starting

### 2. Clone & Point

```bash
git clone https://github.com/xdier/pencil-huashu-design.git
cd pencil-huashu-design
```

Tell your Agent to read the skill:

```
Read ./SKILL.md and follow the design workflow
```

### 3. Design

Give a prompt:

```
Design a landing page for my AI writing tool "WriteFlow". 
Target: content creators and marketing teams. 
Style: clean, professional, not flashy.
```

The Agent will:
1. Fill a Design Brief and ask you to confirm
2. Plan the component structure and design tokens
3. Build the design in Pencil (with screenshots at each step)
4. Run a 5-dimension quality review and fix issues
5. Deliver the .pen file

---

## How It Works

```
Your prompt
    ↓
Agent reads SKILL.md (150 lines, progressive loading)
    ↓
Stage A: Confirm Brief (waits for your OK)
    ↓
Stage B: Plan structure + design tokens + style direction
    ↓
Stage C: Build in Pencil (batch_design → screenshot → fix → repeat)
    ↓
Stage D: 5-dimension review → anti-slop check → deliver
```

Key design decisions:
- **Design Brief Gate** — Agent never starts without your confirmation
- **Fallback Mode** — When you don't know what style you want, Agent proposes 3 directions
- **Anti-AI-Slop System** — 50+ checklist items in two tiers (hard-banned vs needs-justification)
- **Brand Asset Protocol** — 5-step process for real brand identities
- **Error Recovery** — Graceful handling of MCP failures, mid-flow changes, missing assets

---

## Core Methodology

### Anti-AI-Slop (Two-Tier System)

**Hard-banned** (fix immediately):
- Purple/rainbow gradients
- Neon accent colors (#00D4FF, #39FF14)
- Emoji as icons
- Tech-grid backgrounds

**Needs justification** (context-dependent):
- Blue primary (#3B82F6) — OK for SaaS, but not because "AI default"
- 3-column equal grid — OK for dashboards, but not because "lazy layout"
- Inter/Roboto display — OK for docs, but not because "most familiar font"

### 5 Design Schools (with Executable Parameters)

| School | Vibe | Best For |
|--------|------|----------|
| **Pentagram** | Precise, grid-based, restrained | SaaS, developer tools |
| **Field.io** | Tactile, warm, glassmorphism | Consumer hardware, luxury |
| **Kenya Hara** | Minimal, East-Asian, negative space | Culture, art, publishing |
| **Sagmeister** | Bold, experimental, loud | Youth brands, entertainment |
| **Nordic Functionalism** | Warm, human, democratic | Lifestyle, education |

Each school has ready-to-use `set_variables` blocks with concrete hex values, fonts, and spacing. See `references/design-philosophy.md`.

---

## File Structure

```
pencil-huashu-design/
├── SKILL.md                           # Agent reads this (~150 lines)
├── README.md                          # You're reading this
├── CLAUDE.md                          # Claude Code-specific guidance
├── CONTRIBUTING.md                    # How to contribute
├── LICENSE                            # MIT
├── .gitignore
└── references/
    ├── mcp-tools-reference.md         # MCP tool parameters (aligned to actual API)
    ├── anti-slop-checklist.md         # 50+ items, two-tier system
    ├── design-philosophy.md           # 20 schools + 5 executable parameter sets
    ├── quality-review-template.md     # 5-dimension review template
    ├── pre-delivery-checklist.md      # Delivery checklist
    ├── workflow-guide.md              # Step-by-step execution guide
    ├── design-brief-template.md       # Brief template (bilingual)
    └── examples/
        └── saas-landing-page.md       # Full end-to-end example with MCP calls
```

---

## MCP Tools Used

| Tool | Purpose |
|------|---------|
| `batch_design` | Create / modify / copy / move / delete elements |
| `batch_get` | Read design structure and search nodes |
| `get_screenshot` | Render design preview |
| `snapshot_layout` | Detect overlaps, clipping, alignment issues |
| `get/set_variables` | Design token system |
| `export_nodes` | Export to PNG / PDF |
| `search_all_unique_properties` | Audit colors and fonts in use |

Full parameter reference: `references/mcp-tools-reference.md`

---

## Credits

- **[huashu-design](https://github.com/alchaincyf/huashu-design)** by [alchaincyf](https://github.com/alchaincyf) — Original design skill methodology (anti-AI-slop, Junior Designer workflow, brand asset protocol). This project adapts it from HTML/CSS to Pencil MCP.
- **[Pencil Project](https://pencil.evolus.vn/)** by Evolus — Free, open-source GUI prototyping tool.

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

---

## License

MIT License. See [LICENSE](./LICENSE).

---

*pencil-huashu-design is not affiliated with or endorsed by Pencil / Evolus or huashu-design.*

<!-- GitHub Topics (configure in repo settings after push):
pencil, design-skill, ai-agent, ui-design, design-system, mcp, claude-code, cursor, anti-ai-slop, design-philosophy, prototype, mockup
-->
