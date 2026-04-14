---
technique: "Constraints before guidelines"
when: "Every section that mixes rules and preferences. Order is enforced — never interleave them."
---

# Test 04 — Constraints before guidelines

## Technique

> Within any section, hard rules appear before soft preferences.
> **When:** Every section that mixes rules and preferences. Order is enforced — never interleave them.

## Why this test exists

Users naturally mix hard constraints and soft preferences in no particular order. The skill must identify which rules are hard (absolute prohibitions/requirements) and which are soft (style, performance suggestions), then reorder them — hard rules first, preferences after. The input deliberately interleaves both types.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an API response rules document for my backend agents.

Rules for writing API responses:
- Use camelCase for all JSON keys
- Never return HTTP 200 with an error body
- Keep response payloads under 1MB where possible
- Never omit the request_id field in any response
- Include a meta object with pagination info on list endpoints
- Never expose internal stack traces in error responses
```

## What to observe

All 6 rules are about the same topic (API responses) — the skill cannot split them into separate semantic sections. Within the single section, hard constraints and soft preferences are interleaved:

| Rule | Type | Why |
|---|---|---|
| Use camelCase for all JSON keys | Soft preference | Style — no correctness consequence |
| Never return HTTP 200 with an error body | Hard constraint | Correctness — breaks clients |
| Keep response payloads under 1MB | Soft preference | Performance — no immediate breakage |
| Never omit request_id in any response | Hard constraint | Traceability — breaks observability |
| Include meta object on list endpoints | Soft preference | Convention — no security consequence |
| Never expose internal stack traces | Hard constraint | Security — leaks internals |

Expected output order within the section: hard constraints (rules 2, 4, 6) first — then soft preferences (rules 1, 3, 5).

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Hard constraints appear first | All "never" / absolute rules appear before any preference or guideline within the section |
| C2 | No interleaving | Hard rules and soft preferences are not mixed — each group is contiguous |
| C3 | No reclassification | No hard rule demoted to a preference, no preference promoted to a hard rule |
| C4 | All 7 rules present | No rule dropped during reordering |

## Fail signals

- Any soft preference appears before a hard constraint in the same section
- Hard and soft rules are interleaved in the output (even if partially reordered)
- A "never" rule is softened or moved to a note/comment
- Rules are split across separate sections to avoid reordering (structural workaround)
