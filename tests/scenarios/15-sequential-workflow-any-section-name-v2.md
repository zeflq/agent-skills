---
technique: "Strict sequential workflow"
when: "Any multi-step process where sequence matters — regardless of the section's XML tag name."
---

# Test 17 — Strict sequential workflow in non-`<steps>` sections (v2)

## Technique

> Workflow steps are numbered. Order is enforced.
> **When:** Any multi-step process where sequence matters — regardless of the section's XML tag name.

## Why this test exists

Test 15 used `<rollback>` which was the exact section that failed in the deployment runbook. This test uses an `<onboarding>` section to confirm the agent applies strict sequential workflow universally — not just in sections it has seen before.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an agent onboarding doc for new engineers joining the team.

Environment setup (must be done in this exact order):
- Install Node.js 20 and pnpm
- Clone the repository and run pnpm install
- Copy .env.example to .env and fill in the required values
- Run pnpm dev and confirm the app starts on localhost:3000
```

## What to observe

The section will be named `<environment-setup>` or `<onboarding>` — not `<steps>`. The input explicitly states "must be done in this exact order":
- Node.js must be installed before pnpm install (pnpm needs Node)
- pnpm install must run before .env setup (repo must exist first)
- .env must exist before running dev (app needs env vars)
- Dev server must start to confirm everything worked

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Numbered `<step>` elements used | Section contains `<step number="1">`, `<step number="2">` etc. — not `<rule>` elements |
| C2 | All 4 steps present in order | Install → Clone → .env → dev server, no steps dropped or reordered |
| C3 | Technique applied despite section name | Section is NOT named `<steps>` — strict sequential workflow still enforced |
| C4 | Each step has a concrete example | Every `<step>` includes an `<example>` |

## Fail signals

- Section uses `<rule>` elements instead of numbered `<step>` elements
- Steps are present but unnumbered
- Agent only uses `<step>` when section is named `<steps>`, `<deployment>`, or `<rollback>`
