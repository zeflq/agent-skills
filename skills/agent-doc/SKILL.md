---
name: agent-doc
description: "Write any agent-readable document — instructions, architecture guides, runbooks, API contracts, style guides, or any .md file an agent will read as context. Use when a user needs to create a structured document that an AI agent will consume: frontend architecture doc, deployment runbook, database schema guide, API usage rules, etc. Interviews the user to discover sections, applies XML structure and agent-optimized techniques, adds description frontmatter for pi-context lazy loading, and writes the file."
---

# agent-doc Skill

Read `references/interview.md` for how to discover sections for any document type.
Read `references/techniques.md` when writing any section — apply each technique whose `<when>` condition matches the current context.
Read `references/template.md` for the base document structure.

## Context Detection (before interview)

Check if the user has provided context: a project description, existing docs, README, or file structure.
- If yes: auto-derive document type, likely sections, and any obvious content. Do not ask for what is clear.
- If no: start with the three discovery questions in `references/interview.md`.

## Mode Detection

Check if the target file already exists.

**CREATE mode** — file does not exist: run discovery interview, then write.

**UPDATE mode** — file exists: read it, show one-line summary per section, ask which section to update.

## Interview Rules

- One question at a time.
- Propose a concrete example for each section — ask the user to adapt or confirm.
- If the user says "skip": include the section as an XML comment block.
- Always add `description:` frontmatter — ask: *"In one sentence, when should an agent load this file?"*

## Output

- Write to the path the user specifies (e.g. `./docs/frontend.md`, `./.pi/api-rules.md`)
- Always include `description:` frontmatter at the top
- Wrap every section in named XML tags
- After writing, confirm: *"[filename] written — Sections: [list]."*
