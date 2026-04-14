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
1. Auto-fill sections derivable from it
2. Skip those sections in the interview

ELSE IF user provides nothing:
1. Run the full interview

## Mode Detection

Determine the mode before doing anything else:

| User said | Action |
|---|---|
| "update", "edit", "modify", "fix" / gave a file path / pasted file content | → **UPDATE workflow** |
| "create", "generate", "write", "new" / gave no file | → **CREATE workflow** |

IF unsure:
1. Check if `AGENTS.md` or `CLAUDE.md` exists at the project root → **UPDATE workflow**
2. If neither exists → **CREATE workflow**

---

### CREATE workflow
Walk sections 1–7 (required), then 8–10 (recommended). Write to `./AGENTS.md`.

### UPDATE workflow
1. Load the file:
   - IF file path provided → read from that path
   - ELSE IF content pasted → use it directly, skip disk read
   - ELSE → read `AGENTS.md` or `CLAUDE.md` from project root

2. IF file not found: *"I couldn't find AGENTS.md. Can you confirm the path, or should I create one from scratch?"* — stop here.

3. IF standard file (sections match the skill template: Project Scope, Permissions, etc.):
   1. Show a one-line summary per section
   2. Ask which section to update
   3. Run that section's interview
   4. Rewrite the section

4. ELSE IF custom file (sections don't match the template):
   1. Ask: *"This file uses a custom structure. Do you want to: (A) improve the existing sections using skill writing techniques, or (B) restructure the content into the standard AGENTS.md format?"*
   2. IF user chooses A:
      1. Keep sections as-is
      2. Apply techniques from `references/techniques.md` to each section
      3. Never add or remove sections
   3. ELSE IF user chooses B:
      1. Run the standard section interview
      2. Map existing content into the template
      3. Write the full file

## Interview Rules

<!-- hard constraints first -->
- Never bundle multiple sections in one message.
- Never leave soft language in any rule — apply imperative rewrites before writing.

<!-- soft workflow rules after -->
- If the user gives insufficient info: ask one focused follow-up with a concrete example.
- If the user says **"skip"** or **"not needed"**: include the section as an XML comment block.
- Sections 1–7 are required. Sections 8–10 are recommended.

### Constraints before guidelines — enforce within every section

Before writing any section, classify every item:
- **Hard** — contains `never`, or breaking it causes a security, correctness, or data-integrity failure
- **Soft** — everything else: style, performance, convention, "include X", "use X", "keep X under Y"

Write hard items first, soft items after. Never follow input order if it mixes both.

IF a section mixes both:
1. Reorder — hard rules first, soft rules after — never follow the input's original order
2. Never split into sub-sections to avoid reordering — keep them in one block
3. Never interleave — all hard rules contiguous, then all soft rules contiguous

Correct:
```xml
<rules>
  <!-- hard constraints first -->
  <rule>No `any` type — use `unknown` and narrow.</rule>
  <!-- soft preferences after -->
  <rule>Reuse existing utilities before creating new ones.</rule>
</rules>
```

Forbidden:
```xml
<rules>
  <hard-constraints><rule>No `any` type — use `unknown` and narrow.</rule></hard-constraints>
  <guidelines><rule>Reuse existing utilities before creating new ones.</rule></guidelines>
</rules>
```

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

## Verification — run before confirming the file is complete

- [ ] Sections 1–7 are all present or have an XML comment explaining why each was skipped
- [ ] No section has sub-sections to separate hard from soft rules — one flat block only
- [ ] Hard rules appear before soft rules in every mixed section
- [ ] Every rule uses imperative language — no "try to", "prefer", "should", "where possible"
- [ ] `description:` frontmatter is present in the AGENTS.md file
