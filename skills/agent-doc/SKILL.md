---
name: agent-doc
description: "Write or update any agent-readable document — instructions, architecture guides, runbooks, API contracts, style guides, or any .md file an agent will read as context. Use when a user needs to create, update, edit, or improve a structured document that an AI agent will consume: frontend architecture doc, deployment runbook, database schema guide, API usage rules, etc. Interviews the user to discover sections, applies agent-optimized techniques, adds description frontmatter, and writes the file."
---

# agent-doc Skill

Read `references/interview.md` for how to discover sections for any document type.
Read `references/techniques.md` when writing any section — apply each technique whose `<when>` condition matches the current context.
Read `references/writing-guide.md` when writing any document — apply token budget and split rules.
## Context Detection (before interview)

Never scan the filesystem unprompted.

IF user provides context (project description, pasted config, file content, or scan request):
1. Auto-derive document type, likely sections, and any obvious content
2. Never ask for what is already clear

ELSE IF user provides nothing:
1. Start with the four discovery questions in `references/interview.md`

## Mode Detection

Determine mode before doing anything else:

| User said | Action |
|---|---|
| "update", "edit", "modify", "fix" / gave a file path / pasted file content | → **UPDATE workflow** |
| "create", "generate", "write", "new" / gave no file | → **CREATE workflow** |

IF unsure:
1. File exists at the specified path → **UPDATE workflow**
2. File does not exist → **CREATE workflow**

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
2. Include `description:` frontmatter at the top
3. Apply techniques from `references/techniques.md` — each section uses the technique whose `<when>` matches
4. For any skipped section: add a one-line comment explaining why, e.g. `<!-- rollback: not applicable — read-only service -->`
5. Confirm: *"[filename] written — Sections: [list]."*

## Section Writing Rules — enforce on every section, regardless of how content was provided

Before writing any section, classify every item:
- **Hard** — contains `never`, or breaking it causes a security, correctness, or data-integrity failure
- **Soft** — everything else: style, performance, convention, "include X", "use X", "keep X under Y"

Write hard items first, soft items after. Never follow input order if it mixes both.

IF a section mixes both:
1. Reorder — hard rules first, soft rules after — never follow the input's original order
2. Never split into sub-sections (`<hard-constraints>`, `<guidelines>`, or any named wrapper) to separate them — keep all items in one flat block
3. Never interleave — all hard rules contiguous, then all soft rules contiguous

For sequential steps: never move a step to a different section to "improve" structure — every item the user listed as a step must appear as a numbered `<step>` in `<steps>`, including conditional ones.

Correct:
```xml
<rules>
  <!-- hard constraints first -->
  <rule><requirement>Never return HTTP 200 with an error body.</requirement></rule>
  <!-- soft preferences after -->
  <rule><requirement>Use camelCase for every JSON key.</requirement></rule>
</rules>
```

Forbidden:
```xml
<rules>
  <hard-constraints><rule>Never return HTTP 200 with an error body.</rule></hard-constraints>
  <guidelines><rule>Use camelCase for every JSON key.</rule></guidelines>
</rules>
```

## Verification — run before confirming the document is complete

- [ ] `description:` frontmatter is present and starts with "Load when" or "Use when"
- [ ] Every section uses the technique matching its `<when>` condition in `references/techniques.md`
- [ ] No section has sub-sections (`<hard-constraints>`, `<guidelines>`, or any named wrapper) to separate hard from soft — one flat block only
- [ ] Hard rules appear before soft rules in every mixed section
- [ ] Every rule uses imperative language — no "try to", "prefer", "should", "where possible"
- [ ] Every rule has a concrete `<example>` — no prose explanations
