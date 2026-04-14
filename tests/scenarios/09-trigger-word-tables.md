---
technique: "Trigger word tables"
when: "Any section that routes agent behavior based on user input. Use when the same word in different contexts must always trigger the same action. Covers synonyms."
---

# Test 09 — Trigger word tables

## Technique

> Map keywords or phrases the user might say to a specific action the agent must take.
> Format: 2-column table, left = trigger pattern, right = required action.
> **When:** Any section that routes agent behavior based on user input. Covers synonyms — "delete", "remove", "drop" are one row.

## Why this test exists

Some documents govern agent routing — they tell an agent what to do when it hears specific words. The skill must recognize this pattern and produce a 2-column markdown table, not a prose description or XML list. Synonyms must be collapsed into one row.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a triage routing document for our support bot.

When a user says they want to cancel, quit, stop their subscription, or unsubscribe — route to the retention team.
When they say the product is broken, not working, down, or has a bug — route to the engineering escalation queue.
When they ask for a refund, want their money back, or dispute a charge — route to the billing team.
When they ask how something works, want documentation, or need help understanding a feature — route to the knowledge base.
When they say they want to upgrade, get more seats, or expand their plan — route to the sales team.
```

## What to observe

Each sentence maps a set of trigger synonyms to one routing action. The skill must:
- Produce a 2-column markdown table (trigger pattern → action)
- Collapse synonyms into one row per action
- Not use XML for this section (2 flat columns, all rows consumed together — tables apply)
- Not produce prose paragraphs

| Synonyms | Expected row |
|---|---|
| cancel / quit / stop / unsubscribe | → retention team |
| broken / not working / down / bug | → engineering escalation |
| refund / money back / dispute charge | → billing team |
| how it works / documentation / help / understand feature | → knowledge base |
| upgrade / more seats / expand plan | → sales team |

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | 2-column markdown table | Output contains a markdown table with exactly 2 columns: trigger patterns and action |
| C2 | All 5 routes present | All 5 routing destinations appear in the table |
| C3 | Synonyms collapsed | Each routing destination appears in exactly one row (synonyms not split across rows) |
| C4 | No XML used | The trigger table is not wrapped in XML elements |
| C5 | No prose routing | Routing logic is not described in paragraphs — the table is the sole routing spec |

## Fail signals

- Each synonym gets its own row (table has 15+ rows instead of 5)
- Routing described in prose paragraphs instead of a table
- XML used for the trigger-to-action mapping
- A routing destination appears in more than one row
- Table has more than 2 columns
