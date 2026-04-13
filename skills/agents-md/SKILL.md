---
name: agents-md
description: "Guided creation and update of AGENTS.md — the unified project instruction file recognized by Pi (AGENTS.md) and Claude Code (CLAUDE.md or AGENTS.md). Use when a user asks to create, generate, write, or update an AGENTS.md or CLAUDE.md file for their project. Covers: project scope, permissions, hard constraints, coding standards, tooling, workflow, verification, output expectations, safety, and agent roles."
---

# agents-md Skill

This skill produces a **fixed 10-section AGENTS.md** — an opinionated best-practice structure. Sections 1–7 are required, 8–10 are recommended. It does not discover sections from the project; use `agent-doc` for freeform agent documents.

Read `references/sections.md` for per-section interview prompts and XML output templates — ask the user the plain-text question, then write the XML from their answer.
Read `references/techniques.md` when writing any section — apply each technique whose `<when>` condition matches the current context.
Read `references/writing-guide.md` only if the user asks about maintenance, token budget, or file organization.

## Context Detection (before interview)

Never scan the filesystem unprompted.

IF user provides context (project description, pasted config, file content, or scan request):
- Auto-fill sections derivable from it
- Skip those sections in the interview

ELSE (user provides nothing):
- Run the full interview

## Mode Detection

Determine the mode before doing anything else:

| User said | Action |
|---|---|
| "update", "edit", "modify", "fix" / gave a file path / pasted file content | → **UPDATE workflow** |
| "create", "generate", "write", "new" / gave no file | → **CREATE workflow** |

IF unsure:
- `AGENTS.md` or `CLAUDE.md` exists at the project root → **UPDATE workflow**
- Does not exist → **CREATE workflow**

---

### CREATE workflow
Walk sections 1–7 (required), then 8–10 (recommended). Write to `./AGENTS.md`.

### UPDATE workflow
1. Load the file:
   - File path provided → read from that path
   - Content pasted → use it directly, skip disk read
   - No path/content → read `AGENTS.md` or `CLAUDE.md` from project root

2. If file not found: *"I couldn't find AGENTS.md. Can you confirm the path, or should I create one from scratch?"* — stop here.

3. **Standard file** (sections match the skill template: Project Scope, Permissions, etc.)
   → Show a one-line summary per section, ask which to update, run that section's interview, rewrite.

4. **Custom file** (sections don't match the template)
   → Ask: *"This file uses a custom structure. Do you want to: (A) improve the existing sections using skill writing techniques, or (B) restructure the content into the standard AGENTS.md format?"*
   - **(A)** Keep sections as-is, apply techniques from `references/techniques.md` to each one. Do not add or remove sections.
   - **(B)** Run the standard section interview, map existing content into the template, write the full file.

## Interview Rules

- One section at a time. Never bundle multiple sections in one message.
- If the user gives insufficient info: ask one focused follow-up with a concrete example.
- If the user says **"skip"** or **"not needed"**: include the section as an XML comment block.
- Sections 1–7 are required. Sections 8–10 are recommended.

## Section Order

| # | Section | Status |
|---|---|---|
| 1 | Project Scope | Required |
| 2 | Permissions & Boundaries | Required |
| 3 | Hard Constraints | Required |
| 4 | Coding Standards | Required |
| 5 | Tooling & Commands | Required |
| 6 | Default Workflow | Required |
| 7 | Verification Checklist | Required |
| 8 | Output Expectations | Recommended |
| 9 | Safety Rules | Recommended |
| 10 | Agent Roles | Optional |

## Output

1. Write the completed file to `./AGENTS.md` in the working directory root
2. Confirm: *"AGENTS.md written — Sections filled: [list]. Skipped: [list]."*
