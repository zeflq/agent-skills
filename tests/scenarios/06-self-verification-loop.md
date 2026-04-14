---
technique: "Self-verification loop"
when: "Any workflow that produces output the agent could get wrong silently. Place at the end of the workflow."
---

# Test 06 — Self-verification loop

## Technique

> Checklist forces the agent to verify its own work before responding.
> **When:** Any workflow that produces output the agent could get wrong silently. Place at the end of the workflow.

## Why this test exists

Documents that guide agents through a workflow need a final checklist the agent runs against its own output. Without it, silent errors (missing fields, wrong format) go undetected. The skill must append a `<verification>` block with actionable yes/no checks — not a summary of what was done.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a runbook for creating a new database migration for our Django app.

Steps:
- Create the migration file using manage.py makemigrations
- Review the generated migration for correctness
- Write a reverse migration in the operations list
- Run the migration locally and verify with manage.py showmigrations
- Add the migration to the PR and note any data-loss risk in the PR description
```

## What to observe

The workflow produces output an agent could silently get wrong:
- Missing reverse migration
- Data-loss risk not flagged
- Migration file not committed

The skill must:
- Include a `<verification>` block at the end
- Each check must be a yes/no question or checkbox the agent can tick
- Checks must cover the failure modes, not just restate the steps

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | `<verification>` block present | Output ends with a `<verification>` XML block |
| C2 | Verification is a checklist | Items are checkboxes (`- [ ]`) or explicit yes/no questions |
| C3 | Covers failure modes | At least one check addresses reverse migration, one addresses data-loss risk |
| C4 | Placed at the end | `<verification>` appears after all workflow steps, not inline |
| C5 | Actionable items | No item is "summarize what you did" — each item is verifiable (e.g., "reverse migration exists and is runnable") |

## Fail signals

- No `<verification>` block at all
- Verification items restate steps ("did you create the migration?") instead of checking outcomes
- Verification placed mid-document instead of at the end
- Items cannot be answered yes/no (e.g., "ensure quality")
