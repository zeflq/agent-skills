# AGENTS.md Writing Guide

Read this file when the user asks about maintaining, organizing, or trimming their AGENTS.md.
Do not load this during the normal interview — it is not part of the output file.

---

## Token Budget

| Level | Recommended max |
|---|---|
| Root AGENTS.md | 80–120 lines |
| Per section | 10–15 lines |
| Total if using linked files (pi-context) | 300 lines across all loaded files |

**Cut:** explanatory prose, redundant examples, speculative rules, empty XML blocks.
**Keep:** permissions model, hard constraints, workflow steps, verification checklist.

---

## When to Split Into Linked Files (pi-context)

Use `.pi/agents.md` as root + linked stubs when:
- The file exceeds 120 lines
- Some sections are rarely relevant (e.g. Agent Roles for a solo project)
- You use pi-context's context-graph extension

Every linked file needs a `description:` frontmatter field so the agent decides when to pull it.

**What stays in root (always loaded):**
- Hard Constraints
- Permissions & Boundaries
- Default Workflow

**What can be a stub (loaded on demand):**
- Coding Standards (read when writing code)
- Tooling & Commands (read when running commands)
- Output Expectations
- Safety Rules
- Agent Roles

---

## Violation-Driven Iteration

Add rules only after real failures. Never speculatively.

**Workflow:** Agent makes a mistake → identify the exact decision point → add one specific rule → include the real example as evidence.

```xml
<!-- Example of a learned rule -->
<learned-rules>
  <rule date="YYYY-MM-DD" title="[What went wrong]">
    <symptom>[What the agent did and what broke]</symptom>
    <rule>[The specific constraint that prevents recurrence]</rule>
  </rule>
</learned-rules>
```

---

## Staleness Check

Run on every major refactor:

| Question | Action if yes |
|---|---|
| Are any specific file paths documented? | Replace with capability descriptions |
| Are any rules longer than 3 lines? | Cut to the essential constraint |
| Has any section gone untouched for 3+ months? | Delete or archive it |
| Is the file longer than 120 lines? | Apply token budget and trim |
| Do any rules contradict each other? | Resolve immediately |
