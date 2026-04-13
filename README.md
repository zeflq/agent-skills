# agent-skills

A marketplace of skills for writing agent-readable documents and instructions.

## Skills

### agents-md

Guided creation and update of `AGENTS.md` / `CLAUDE.md` — the unified project instruction file recognized by Claude Code, Pi, and GitHub Copilot.

**Use when:** you want to set up or improve an agent's project-level instructions.

Produces a fixed, opinionated 10-section structure (Project Scope, Permissions, Hard Constraints, Coding Standards, Tooling, Workflow, Verification, Output Expectations, Safety Rules, Agent Roles). Sections 1–7 are required, 8–10 are recommended. The skill interviews you section by section, applies agent-optimized writing techniques, and writes the file.

**Does not** discover sections from the project — the structure is predetermined.

---

### agent-doc

Write any agent-readable `.md` document — architecture guides, runbooks, API contracts, style guides, database schema guides, or any file an agent will load as context.

**Use when:** you need a custom document that an AI agent will read, and the sections aren't predetermined.

Interviews you to discover the right sections for your document type, selects the best technique for each section (XML tags, tables, numbered steps, if-then logic), and writes the file with `description:` frontmatter so agents know when to load it.

**Use instead of agents-md** when you need a freeform document with project-specific sections.

## Compatibility

All skills use the standard `SKILL.md` format and are compatible across tools.

| Platform | Install path | Invoke |
|---|---|---|
| [Claude Code](https://claude.ai/code) | `~/.claude/skills/` or `.claude/skills/` | `/agents-md` |
| [Pi](https://github.com/badlogic/pi-mono) | `~/.agents/skills/` or `.agents/skills/` | `/skill:agents-md` |
| [GitHub Copilot](https://docs.github.com/en/copilot) | `~/.copilot/skills/` or `.github/skills/` | `/agents-md` |
| All three | `~/.agents/skills/` | — |

## Install

**Universal (works for Claude Code, Pi, and Copilot):**
```bash
cp -r skills/agents-md ~/.agents/skills/
cp -r skills/agent-doc ~/.agents/skills/
```

**Pi package (all skills at once):**
```bash
pi install git:github.com/zeflq/agents-md-skill
```

**Claude Code (per skill):**
```bash
claude skill install agents-md.skill
claude skill install agent-doc.skill
```
