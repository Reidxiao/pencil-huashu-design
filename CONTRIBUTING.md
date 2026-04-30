# Contributing to pencil-huashu-design

Thanks for your interest in contributing! This project is a design skill for AI agents — contributions can come in many forms.

## Ways to Contribute

### Report Issues

Found a bug in the skill? Have a suggestion? Open an issue using the templates provided.

### Improve Design Philosophy

The `references/design-philosophy.md` file covers 20 design schools. If you have expertise in a specific school and want to improve the executable parameters (color palettes, font pairs, spacing), PRs are welcome.

### Expand Anti-AI-Slop Checklist

The `references/anti-slop-checklist.md` is a living document. If you spot new AI-slop patterns that aren't covered, or think a rule should be reclassified (hard-ban vs needs-justification), open a PR with examples.

### Add Examples

The `references/examples/` directory needs more end-to-end case studies. If you've used this skill to create a design and want to share the process (anonymized), add your example.

### Fix MCP API References

Pencil MCP tools may evolve. If the `references/mcp-tools-reference.md` is out of date with the actual Pencil MCP API, please update it.

## Guidelines

- **SKILL.md is the source of truth** — changes to workflow logic go in SKILL.md, not in references.
- **References are loaded on-demand** — keep SKILL.md lean (~150 lines), put details in references/.
- **Keep it language-consistent** — SKILL.md and references/ are in Chinese. README.md and CLAUDE.md are in English.
- **Anti-slop rules need evidence** — when adding a new anti-slop rule, include a visual example of the bad pattern and the recommended alternative.
- **Executable parameters must be tested** — when adding `set_variables` blocks, verify they work with the actual Pencil MCP tools.

## Pull Request Process

1. Fork the repo
2. Create a branch (`git checkout -b improve-design-philosophy`)
3. Make your changes
4. Test with a real Pencil MCP session if possible
5. Submit a PR with a clear description of what changed and why

## Code of Conduct

Be respectful. This is a design-focused project — aesthetic disagreements are normal. Back up opinions with examples.
