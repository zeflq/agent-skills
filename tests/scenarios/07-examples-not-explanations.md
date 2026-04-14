---
technique: "Examples not explanations"
when: "Every rule or technique that could be misapplied. One example beats three sentences of explanation."
---

# Test 07 — Examples not explanations

## Technique

> Each section shows a concrete filled-in example. No prose explanations inside the doc.
> **When:** Every rule or technique that could be misapplied. One example beats three sentences of explanation.

## Why this test exists

Users write rules with prose explanations ("This is important because…", "The reason we do this is…"). The skill must strip the prose and replace it with a concrete example inside each `<rule>` element. If the user provides no example, the skill must invent one. Explanations must never appear in the final document.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a naming conventions document for our REST API endpoints.

Rules:
- Use kebab-case for URL path segments. This is important because camelCase in URLs is hard to read and some proxies lowercase URLs anyway.
- Always version the API in the URL path. We've learned from experience that putting the version in a header causes problems with caching and routing.
- Resource names must be plural nouns. The reason is that endpoints represent collections, so singular names are misleading to clients.
- Never use verbs in path segments. HTTP methods already express the action, so adding a verb like /getUser is redundant and confusing.
```

## What to observe

Each rule contains prose explanation mixed in. The skill must:
- Keep the rule itself
- Replace all prose explanation with a concrete code/URL example
- Produce no "because…", "the reason is…", or "this is important because…" prose in the output

| Rule | Prose to remove | Expected example |
|---|---|---|
| kebab-case URLs | "This is important because…" | `/user-profiles/123` not `/userProfiles/123` |
| Version in URL | "We've learned from experience that…" | `/v1/orders` not `?version=1` or `X-API-Version: 1` |
| Plural nouns | "The reason is that…" | `/orders` not `/order`, `/users` not `/user` |
| No verbs | "HTTP methods already express…" | `GET /users/123` not `GET /getUser/123` |

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | No "because" prose | The phrases "because", "the reason", "this is important", "we've learned" do not appear in any rule |
| C2 | Each rule has an `<example>` | Every `<rule>` element contains a concrete `<example>` with a real URL or code snippet |
| C3 | All 4 rules present | No rule dropped or merged |
| C4 | Examples are concrete | Each example contains an actual URL or value — not "for example, use the right format" |
| C5 | Negative example present | At least some examples show what NOT to do (not just the correct form) |

## Fail signals

- Any prose explanation survives into the output
- An `<example>` field contains a sentence ("Use kebab-case like the standard recommends") instead of a real URL
- A rule is dropped because the prose was the only content and the skill didn't invent an example
- Examples are abstract ("e.g., the correct format") rather than concrete ("/user-profiles/123")
