# agent-skills

A marketplace of skills for writing agent-readable documents and instructions.

## Skills

| Skill | Description |
|---|---|
| **agents-md** | Guided creation of `AGENTS.md` / `CLAUDE.md` — the unified project instruction file |
| **agent-doc** | Write any agent-readable document — architecture guides, runbooks, API contracts, style guides |

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
