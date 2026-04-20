---
technique: "Constraints before guidelines"
when: "Every section that mixes rules and preferences — regardless of the section's XML tag name."
---

# Test 14 — Constraints before guidelines in non-`<rules>` sections

## Technique

> Hard rules appear before soft preferences in every section. Hard: contains `never`, requires immediate action, or breaking it causes a security, correctness, or data-integrity failure. Soft: everything else.
> **When:** Every section that mixes hard and soft — regardless of the section's XML tag name.

## Why this test exists

Previous runs showed the agent applies constraint ordering correctly inside `<rules>` blocks but reverts to input order when the section has a domain-specific name like `<post-deployment-checks>` or `<authentication>`. This test uses a non-`<rules>` section name to confirm the technique is applied universally.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an API contract document for my backend.

Authentication rules:
- Include the Authorization header with every request
- Never send requests over HTTP — HTTPS only
- Retry once with a refreshed token if a 401 is returned
- Never expose the token value in logs or error messages
- Use Bearer scheme for token format
```

## What to observe

The section will be named `<authentication>` or similar — not `<rules>`. The input mixes hard and soft items in no particular order:

| Rule | Type | Why |
|---|---|---|
| Include the Authorization header with every request | Soft | Convention — no immediate security failure |
| Never send requests over HTTP | Hard | Security — contains `never` |
| Retry once with a refreshed token on 401 | Soft | Resilience preference |
| Never expose the token value in logs | Hard | Security — contains `never` |
| Use Bearer scheme for token format | Soft | Convention |

Expected output order: both `never` rules first (hard), then the three soft rules after.

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Hard rules appear first | Both "Never send over HTTP" and "Never expose token in logs" appear before any soft rule |
| C2 | No interleaving | All hard rules are contiguous, all soft rules are contiguous |
| C3 | Ordering applies despite section name | The section is NOT named `<rules>` — constraint ordering still enforced |
| C4 | All 5 rules present | No rule dropped during reordering |

## Fail signals

- Any soft rule appears before a hard rule in the section
- Hard and soft rules are interleaved
- The agent only reorders when the section is named `<rules>`
