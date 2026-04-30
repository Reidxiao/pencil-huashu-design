# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **pencil-huashu-design** — an AI Agent skill for professional UI design using Pencil MCP tools. It is NOT a code project with build/test/lint steps. It is a documentation and skill definition repository that guides AI agents (Claude Code, Cursor, etc.) through a structured design workflow.

The skill adapts [huashu-design](https://github.com/alchaincyf/huashu-design) principles (anti-AI-slop, brand asset protocol, 5-dimension review) from HTML/CSS output to Pencil's native component system.

## Architecture

```
Human prompt → AI Agent (reads SKILL.md) → Pencil MCP Tools → Pencil Canvas (.pen file)
```

- **SKILL.md** — Primary skill document (~150 lines). Agent reads this to execute the design workflow. Uses progressive loading: references specific files at each stage trigger point.
- **references/** — Supporting documentation loaded on-demand:
  - `mcp-tools-reference.md` — Complete MCP tool parameter reference aligned to actual Pencil MCP API
  - `anti-slop-checklist.md` — 50+ item checklist with two-tier system (hard-banned vs needs-justification)
  - `design-philosophy.md` — 20 design philosophies; top 5 have executable `set_variables` parameter blocks
  - `quality-review-template.md` — 5-dimension review template with MCP verification commands
  - `pre-delivery-checklist.md` — Delivery checklist including naming convention checks
  - `workflow-guide.md` — Detailed step-by-step execution guide with correct MCP call syntax
  - `design-brief-template.md` — Design brief template (Chinese/English bilingual)
  - `examples/saas-landing-page.md` — End-to-end example with full MCP calls from prompt to delivery

## Key Technical Details

- **All MCP tool examples use actual Pencil MCP API** — `batch_design` uses JavaScript syntax (`I()`, `U()`, `C()`, `M()`, `D()`, `R()` operations), not JSON. `batch_get` uses `patterns`/`nodeIds`/`readDepth`, not `action`. `get_screenshot` requires `nodeId`. `snapshot_layout` uses `maxDepth`/`problemsOnly`, not `action`.
- **Anti-slop rules are layered** — Hard-banned items (purple gradients, emoji icons) are always forbidden. Context-dependent items (blue primary, 3-column grid) require the agent to self-check "why am I choosing this?" before using.
- **Design philosophy has executable parameters** — The 5 main schools (Pentagram, Field.io, Kenya Hara, Sagmeister, Nordic) each include a ready-to-use `set_variables` block with concrete hex values, fonts, spacing, and radius.
- **SKILL.md uses progressive loading** — It references specific files at each stage rather than embedding all content. Agent should read referenced files when reaching each stage trigger point.
- **Component naming convention** — `{function}-{level}` pattern: `hero-title`, `feature-card`, `nav-link`. Not `card1`, `card2`, `card-copy-3`.
- **Error recovery is built in** — SKILL.md includes handling for MCP failures, mid-flow requirement changes, missing brand assets, and unsatisfying design directions.

## File Conventions

- All reference docs and SKILL.md are in Chinese. README.md and CLAUDE.md are in English.
- `output_language: follow user's language` directive in SKILL.md frontmatter.
- Commit message format: `feat(design): ...`, `fix(design): ...`, `refactor(design): ...`
