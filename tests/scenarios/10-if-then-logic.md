---
technique: "If-then logic"
when: "Any decision the agent must make consistently. If a branch exceeds 4 steps, move it to a linked file. Never use prose for decisions."
---

# Test 10 — If-then logic

## Technique

> Write conditional rules as explicit IF / ELSE IF branches with numbered steps. Never use prose for decisions. Each branch ends with a concrete action, never "use judgment".
> **When:** Any decision the agent must make consistently. If a branch exceeds 4 steps, move it to a linked file.

## Why this test exists

Users describe conditional logic in prose ("If X, then maybe consider Y, but it depends…"). The skill must convert every decision point into explicit IF / ELSE IF branches with numbered steps. No branch may end with "use judgment" — each must end in a concrete, unambiguous action.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an incident severity classification guide for our on-call agents.

Classification rules:
- If the issue affects all users and revenue is impacted, it's a SEV1 — page the on-call lead immediately and open a war room.
- If some users are affected but core functionality still works, it's a SEV2 — notify the on-call engineer and create a ticket.
- If it only affects a small subset of users or it's a cosmetic issue, it's a SEV3 — create a ticket and triage in the next standup.
- If you're not sure, escalate to SEV2 by default and let the on-call engineer downgrade it.
- If monitoring is down and you can't tell the impact, treat it as SEV1 until confirmed otherwise.
```

## What to observe

Five distinct conditional branches. The skill must:
- Produce explicit IF / ELSE IF blocks (not prose paragraphs)
- Each branch must have numbered steps (even if just 1 step)
- No branch ends with "use judgment" or "assess the situation"
- The uncertainty and monitoring-down branches must be explicit conditions, not footnotes

| Condition | Expected branch |
|---|---|
| All users + revenue impacted | IF → SEV1: page lead + open war room |
| Some users, core works | ELSE IF → SEV2: notify engineer + create ticket |
| Small subset or cosmetic | ELSE IF → SEV3: create ticket + triage at standup |
| Uncertain impact | ELSE IF → escalate to SEV2 by default |
| Monitoring down | ELSE IF → treat as SEV1 until confirmed |

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | IF/ELSE IF structure used | Output uses explicit IF / ELSE IF labels (not prose "in this case…") |
| C2 | All 5 branches present | All 5 conditions appear as explicit branches |
| C3 | Each branch has numbered steps | Steps within each branch are numbered, not bulleted |
| C4 | No "use judgment" endings | No branch ends with "assess the situation", "use judgment", or "decide based on context" |
| C5 | Uncertainty is an explicit branch | The "not sure" case is a named ELSE IF, not a footnote or prose aside |
| C6 | Monitoring-down is an explicit branch | The monitoring-down case is a named ELSE IF with a concrete action |

## Fail signals

- Conditions described as prose paragraphs instead of IF/ELSE IF blocks
- Any branch ends with "use judgment" or "escalate as appropriate"
- The uncertainty or monitoring-down cases are footnotes rather than named branches
- Steps within a branch are bulleted instead of numbered
- Branches collapsed or merged (e.g., SEV2 and SEV3 into one "lower severity" branch)
