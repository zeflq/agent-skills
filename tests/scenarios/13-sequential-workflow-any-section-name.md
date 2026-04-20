---
technique: "Strict sequential workflow"
when: "Any multi-step process where sequence matters — regardless of the section's XML tag name."
---

# Test 15 — Strict sequential workflow in non-`<steps>` sections

## Technique

> Workflow steps are numbered. Order is enforced.
> **When:** Any multi-step process where sequence matters — regardless of the section's XML tag name.

## Why this test exists

Previous runs showed the agent uses numbered `<step>` elements inside sections named `<steps>` or `<deployment>`, but reverts to `<rule>` elements in sections like `<rollback>` or `<recovery>` — even when those sections describe an ordered sequence. This test uses a non-`<steps>` section name to confirm the technique is applied universally.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a database migration runbook.

Rollback procedure (must be done in this exact order):
- Restore the database from the pre-migration snapshot
- Restart the application servers pointing to the restored database
- Verify the application health endpoint returns 200
- Notify the on-call engineer that rollback is complete
```

## What to observe

The section will be named `<rollback>` — not `<steps>`. The input explicitly states "must be done in this exact order" — sequence matters. Each item is an action that must follow the previous one:
- Restore snapshot must happen before restarting servers (servers need the DB)
- Restart must happen before health check (nothing to check until servers are up)
- Health check must pass before notifying (don't notify until confirmed recovered)

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Numbered `<step>` elements used | `<rollback>` contains `<step number="1">`, `<step number="2">` etc. — not `<rule>` elements |
| C2 | All 4 steps present in order | Restore → Restart → Verify → Notify, no steps dropped or reordered |
| C3 | Technique applied despite section name | Section is NOT named `<steps>` — strict sequential workflow still enforced |
| C4 | Each step has a concrete example | Every `<step>` includes an `<example>` showing what execution looks like |

## Fail signals

- `<rollback>` uses `<rule>` elements instead of numbered `<step>` elements
- Steps are present but unnumbered
- The agent only uses `<step>` elements when the section is named `<steps>` or `<deployment>`
