---
name: agent-doc
description: "Write any agent-readable document — instructions, architecture guides, runbooks, API contracts, style guides, or any .md file an agent will read as context. Use when a user needs to create a structured document that an AI agent will consume: frontend architecture doc, deployment runbook, database schema guide, API usage rules, etc. Interviews the user to discover sections, applies agent-optimized techniques, adds description frontmatter, and writes the file."
---

# agent-doc Skill

Read `references/interview.md` for how to discover sections for any document type.
Read `references/techniques.md` when writing any section — apply each technique whose `<when>` condition matches the current context.
## Context Detection (before interview)

Never scan the filesystem unprompted.

IF user provides context (project description, pasted config, file content, or scan request):
- Auto-derive document type, likely sections, and any obvious content
- Do not ask for what is already clear

ELSE (user provides nothing):
- Start with the four discovery questions in `references/interview.md`

## Mode Detection

Determine mode before doing anything else:

| User said | Action |
|---|---|
| "update", "edit", "modify", "fix" / gave a file path / pasted file content | → **UPDATE workflow** |
| "create", "generate", "write", "new" / gave no file | → **CREATE workflow** |

IF unsure:
- File exists at the specified path → **UPDATE workflow**
- File does not exist → **CREATE workflow**

---

### CREATE workflow
Run the discovery interview (`references/interview.md`), then write.

### UPDATE workflow
1. Load the file:
   - File path provided → read from that path
   - Content pasted → use it directly, skip disk read
   - No path/content → ask: *"What file should I update?"*

2. If file not found: *"I couldn't find that file. Can you confirm the path, or should I create it from scratch?"* — stop here.

3. Show a one-line summary per section, ask which to update, run that section's interview, rewrite.

## Output

1. Write to the path the user specifies (e.g. `./docs/frontend.md`, `./.pi/api-rules.md`)
3. Include `description:` frontmatter at the top
4. Apply techniques from `references/techniques.md` — each section uses the technique whose `<when>` matches
5. For any skipped section: add a one-line comment explaining why, e.g. `<!-- rollback: not applicable — read-only service -->`
6. Confirm: *"[filename] written — Sections: [list]."*
