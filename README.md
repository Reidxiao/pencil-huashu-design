# pencil-huashu-design

> An AI Agent skill for designing with Pencil MCP — built with design philosophy from huashu-design

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

A production-ready design skill for AI coding agents (Claude Code, Cursor, etc.) that enables professional-grade UI design directly in Pencil. Based on the design principles of [huashu-design](https://github.com/alchaincyf/huashu-design) and adapted for Pencil's native component system.

---

## 🎯 What is this?

This is a **Pencil MCP design skill** — not a plugin, not a template, but a systematic design workflow that guides an AI Agent to:

- Design production-ready UI in Pencil from a single prompt
- Apply professional design principles (anti-AI-slop, brand asset protocol, 5-dimension review)
- Use Pencil's native features correctly (reusable components, slots, instances)
- Deliver .pen files that export to React / Vue / HTML / PNG / PDF

---

## 🏗 Architecture

```
Human prompt
     ↓
AI Agent (reads SKILL.md)
     ↓
Pencil MCP Tools (batch_design / batch_get / get_screenshot / etc.)
     ↓
Pencil Canvas (.pen file)
```

- **Agent** is the operator — calls MCP tools to manipulate the canvas
- **Human** only provides the initial prompt, no intermediate involvement
- **SKILL.md** is the Agent's design guideline

---

## ✨ Features

- **4-Stage Design Flow** — Confirm → Plan → Build → Deliver
- **Design Brief Gate** — Never start without a confirmed brief
- **Fallback Mode** — When intent is vague, Agent proposes 3 directions for user to choose
- **Anti-AI-Slop Checklist** — Detects and fixes 50+ common bad design patterns
- **Brand Asset Protocol** — 5-step process for incorporating brand identity correctly
- **5-Dimension Review** — Weighted quality scoring before delivery
- **Pencil Native Components** — Correct usage of `reusable`, `slots`, `instances`, `descendants`
- **Design Kits Integration** — Leverage Pencil's built-in component library first

---

## 🚀 Quick Start

### Prerequisites

- [Pencil](https://pencil.evolus.vn/) installed
- Pencil MCP server running (or Pencil VS Code extension)

### Usage

1. Clone this repo:
```bash
git clone https://github.com/Reidxiao/pencil-huashu-design.git
cd pencil-huashu-design
```

2. Point your Agent to the `SKILL.md` file:
```
Read /path/to/pencil-huashu-design/SKILL.md
```

3. Give your Agent a design prompt:
```
Design a landing page for my AI startup. Clean, modern, with a dark theme.
```

4. Agent will execute the 4-stage workflow and deliver a .pen file.

---

## 📁 File Structure

```
pencil-huashu-design/
├── SKILL.md                          # Main skill document (Agent reads this)
├── README.md                         # This file
├── LICENSE                           # MIT License
└── references/
    ├── mcp-tools-reference.md         # Full MCP tool parameters + JSON examples
    ├── anti-slop-checklist.md         # 50+ item anti-AI-slop checklist
    ├── design-philosophy.md          # 20 design philosophies + 5 schools
    ├── quality-review-template.md     # 5-dimension review template
    └── pre-delivery-checklist.md      # Delivery checklist (expanded)
```

---

## 📐 Core Design Philosophy

### The 4 Schools

| School | Vibe | Best For |
|--------|------|----------|
| **Pentagram School** | Precise, grid-based, restrained | SaaS, developer tools |
| **Field.io School** | Tactile, gradient, glassmorphism | Consumer hardware, luxury |
| **Kenya Hara School** | Minimal, East-Asian, ma (negative space) | Culture, art, publishing |
| **Sagmeister School** | Bold, experimental, loud | Youth brands, entertainment |
| **Nordic Functionalism** | Warm, human, democratic | Lifestyle, education |

### Anti-AI-Slop Core Rules

- ❌ Purple/rainbow gradients
- ❌ Neon blue/green accent (`#00D4FF`, `#39FF14`)
- ❌ Inter/Roboto for display fonts
- ❌ Emoji as icons
- ❌ Rounded cards with left-border accent
- ❌ Perfectly equal 3/4-column grids
- ❌ Pure black/white full-screen background

---

## 🛠 MCP Tools Covered

| Tool | Purpose |
|------|---------|
| `batch_design` | Create/modify/copy/move/delete elements |
| `batch_get` | Read design structure |
| `get_screenshot` | Render design preview |
| `snapshot_layout` | Analyze layout (overlaps, alignment) |
| `get/set_variables` | Design token system |

---

## 📜 Credits

- **huashu-design** — Original design skill philosophy by [alchaincyf](https://github.com/alchaincyf/huashu-design). This project adapts huashu-design's core principles (Junior Designer workflow, anti-AI-slop, brand asset protocol) from HTML/CSS output to Pencil MCP.
- **Pencil** — The [Pencil Project](https://pencil.evolus.vn/) by Evolus, a free open-source GUI prototyping tool.

---

## 📄 License

MIT License. See [LICENSE](./LICENSE) for details.

---

*pencil-huashu-design is not affiliated with or endorsed by Pencil / Evolus or huashu-design.*
