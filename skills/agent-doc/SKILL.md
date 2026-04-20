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

Before writing any section, make two classifications:

**1. Ordered or unordered?**
IF items must be executed in a specific order → use numbered `<step>` elements.
ELSE → use `<rule>` elements.
This applies to any section, regardless of its XML tag name — `<rollback>`, `<setup>`, `<migration>` follow the same rule as `<steps>`.

**2. Hard or soft?**
Classify every item — see the "Constraints before guidelines" technique in `references/techniques.md`.
Write hard items first, soft items after. Never follow input order. Never split into sub-sections.
This applies to any section, regardless of its XML tag name — `<post-deployment-checks>`, `<prerequisites>`, `<rollback>` follow the same rule as `<rules>`.

Correct (`<rollback>` is ordered → use steps):
```xml
<rollback>
  <step number="1"><action>Route traffic back to previous instance.</action></step>
  <step number="2"><action>Redeploy previous stable image.</action></step>
</rollback>
```

Correct (`<post-deployment-checks>` mixes hard and soft → hard first):
```xml
<post-deployment-checks>
  <rule><requirement>Roll back immediately when error rate exceeds 1%.</requirement></rule>
  <rule><requirement>Monitor error rate for 10 minutes after cutover.</requirement></rule>
</post-deployment-checks>
```

## Verification — run before confirming the document is complete

Never confirm the document is complete until every item below passes. If any item fails, rewrite the affected section and re-run the full checklist.

- [ ] `description:` frontmatter is present and starts with "Load when" or "Use when"
- [ ] Every section uses the technique matching its `<when>` condition in `references/techniques.md`
- [ ] No section has sub-sections (`<hard-constraints>`, `<guidelines>`, or any named wrapper) to separate hard from soft — one flat block only
- [ ] Hard rules appear before soft rules in every mixed section
- [ ] Every rule uses imperative language — no "try to", "prefer", "should", "where possible"
- [ ] Every rule has a concrete `<example>` — no prose explanations
- [ ] Every section where sequence matters applies the strict sequential workflow technique from `references/techniques.md`
- [ ] Every document containing a workflow or multi-step process ends with a `<self-verification>` checklist
- [ ] No rule or content block appears more than once across all sections — consolidate by intent, not just exact wording (a rephrased repeat is still a duplicate)
- [ ] No meta-comments appear in the output — the document contains only content, never the skill's internal reasoning
- [ ] No section exceeds 15 lines — if it does, ask the user what to cut before confirming
- [ ] Root file does not exceed 120 lines — if it does, apply token budget from `references/writing-guide.md`
- [ ] Any section exceeding 20 lines, applying to fewer than 30% of tasks, or changing at a different rate is split into a linked file
- [ ] Any permissions section uses an explicit allowlist — nothing permitted by default, no denylists
