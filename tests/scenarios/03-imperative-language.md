---
technique: "Imperative language"
when: "Every rule or constraint in any agent-readable file. Never leave soft language in place."
---

# Test 03 — Imperative language

## Technique

> Rewrite any soft language. "prefer not to" → "never". "try to" → "always".
> **When:** Every rule or constraint in any agent-readable file. Never leave soft language in place.

## Why this test exists

Users naturally write rules with hedging language. The skill must harden every soft phrase into a direct imperative without being told to. The input below contains 7 distinct soft patterns — each one must be rewritten in the output.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a coding standards document for my Python backend.

Rules:
- Try to keep functions under 20 lines
- Prefer not to use global variables
- You should probably add type annotations to all public functions
- Avoid using print() for logging if possible — use the logger instead
- It's recommended to handle exceptions explicitly rather than using bare except
- Where possible, reuse existing utilities instead of writing new ones
- Generally don't commit debug code or commented-out blocks
```

## What to observe

Every rule contains soft language that must be hardened:

| Original phrase | Expected rewrite |
|---|---|
| "Try to keep" | "Never exceed" / "Always keep" |
| "Prefer not to use" | "Never use" |
| "You should probably add" | "Always add" |
| "if possible" | drop qualifier — state the rule directly |
| "It's recommended to" | "Always" |
| "Where possible" | drop qualifier — state the rule directly |
| "Generally don't" | "Never" |

## Checks

Run the evaluator with the generated document and verify:

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | No "try to" | The phrase "try to" does not appear anywhere in the output |
| C2 | No "prefer not to" | The phrase "prefer not to" does not appear anywhere in the output |
| C3 | No hedging qualifiers | "should probably", "recommended", "where possible", "if possible", "generally" do not appear in any rule |
| C4 | Soft rules rewritten to imperatives | Each original soft rule is present as a "never" or "always" equivalent |
| C5 | No rules dropped | All 7 rules from the input are present in the output — none silently removed |

## Fail signals

- Any soft phrase survives unchanged into the output
- A rule is weakened further (e.g., turned into a comment or suggestion)
- A rule is dropped instead of being rewritten
- Imperative rewrite changes the meaning of the rule (e.g., "keep under 20 lines" becomes "keep under 30 lines")
