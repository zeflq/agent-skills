---
technique: "Constraints before guidelines"
when: "Every section that mixes rules and preferences — regardless of the section's XML tag name."
---

# Test 16 — Constraints before guidelines in non-`<rules>` sections (v2)

## Technique

> Hard rules appear before soft preferences in every section. Hard: contains `never`, requires immediate action, or breaking it causes a security, correctness, or data-integrity failure. Soft: everything else.
> **When:** Every section that mixes hard and soft — regardless of the section's XML tag name.

## Why this test exists

Tests 04 and 14 used `<rules>` and `<authentication>` sections. This test uses a `<code-review>` section to confirm the agent applies constraint ordering universally — not just in sections it has seen before.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a coding standards doc for my team.

Code review rules:
- Keep pull requests under 400 lines where possible
- Never approve a PR that skips the test suite
- Add inline comments for non-obvious logic
- Never merge to main without at least one approval
- Prefer small, focused commits over large ones
```

## What to observe

The section will be named `<code-review>` or similar — not `<rules>`. The input mixes hard and soft items:

| Rule | Type | Why |
|---|---|---|
| Keep PRs under 400 lines | Soft | Preference — no correctness failure |
| Never approve a PR that skips the test suite | Hard | Contains `never` — correctness failure |
| Add inline comments for non-obvious logic | Soft | Convention |
| Never merge to main without at least one approval | Hard | Contains `never` — process integrity failure |
| Prefer small, focused commits | Soft | Preference |

Expected output order: both `never` rules first, then the three soft rules after.

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Hard rules appear first | "Never approve" and "Never merge" appear before any soft rule |
| C2 | No interleaving | All hard rules contiguous, all soft rules contiguous |
| C3 | Technique applied despite section name | Section is not named `<rules>` — constraint ordering still enforced |
| C4 | All 5 rules present | No rule dropped during reordering |

## Fail signals

- Any soft rule appears before a hard rule in the section
- Hard and soft rules are interleaved
- Agent only reorders when section is named `<rules>`
