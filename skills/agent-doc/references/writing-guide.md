---
description: Load when writing or updating any agent-readable document. Covers token budget, split rules, and maintenance.
---

# Agent Document Writing Guide

Load when writing any document — apply token budget and split rules. The Staleness Check section applies to maintenance only.

## Token Budget

| Level | Max |
|---|---|
| Root agent doc | 80–120 lines |
| Per section | 10–15 lines |
| Total across all linked files | 300 lines |

| Action | What |
|---|---|
| Cut | Explanatory prose, redundant examples, speculative rules, empty XML blocks |
| Keep | Hard constraints, workflow steps, verification checklist, permissions model |

## When to Split Into Linked Files

<rules>
  <rule>Every linked file must have a <code>description:</code> frontmatter field.</rule>
  <rule>Split a section into a linked file when any condition below is true:
    <conditions>
      <condition>Section exceeds 20 lines</condition>
      <condition>Section applies to fewer than 30% of tasks</condition>
      <condition>Section changes at a different rate than the rest of the file</condition>
    </conditions>
  </rule>
</rules>

| Location | Sections |
|---|---|
| Root (always loaded) | Hard constraints, permissions and boundaries, default workflow |
| Stub (loaded on demand) | Detailed reference tables, examples library, rollback procedures, edge-case rules |

## Multi-File Document Systems

Apply when building a folder of related agent docs (e.g. `/docs/api.md`, `/docs/auth.md`, `/docs/schema.md`).

<rules>
  <rule>Each doc owns exactly one domain — never let two docs govern the same topic.</rule>
  <rule>Never copy rules across docs — link to the doc that owns them instead.</rule>
  <rule>Every doc must have a <code>description:</code> frontmatter field scoped to its own domain.</rule>
  <rule>Create one root index doc when the system has 3+ docs — it describes the system and links to each doc. It owns no domain rules itself.</rule>
</rules>

| Cross-reference pattern | Example |
|---|---|
| Link to owning doc | `See [auth.md](auth.md) for token rules.` |
| Never inline foreign rules | Copy-paste from another doc is forbidden |

## Violation-Driven Iteration

Never add rules speculatively — only after a real failure.

<steps>
  <step number="1">Agent makes a mistake — record exactly what it did.</step>
  <step number="2">Identify the exact decision point where it went wrong.</step>
  <step number="3">Add one specific rule targeting that decision point.</step>
  <step number="4">Include the real failure as an example inside the rule.</step>
</steps>

```xml
<learned-rules>
  <rule date="YYYY-MM-DD" title="[What went wrong]">
    <symptom>[What the agent did and what broke]</symptom>
    <rule>[The specific constraint that prevents recurrence]</rule>
  </rule>
</learned-rules>
```

## Staleness Check

Run on every major update to the system the document describes.

| Question | Action if yes |
|---|---|
| Are any specific file paths hardcoded? | Replace with capability descriptions |
| Are any rules longer than 3 lines? | Cut to the essential constraint |
| Has any section gone untouched for 3+ months? | Delete or archive it |
| Is the file longer than 120 lines? | Apply token budget and trim |
| Do any rules contradict each other? | Resolve immediately |
