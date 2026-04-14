---
technique: "description frontmatter"
when: "Every file that may be stub-loaded by pi-context. Without it, the LLM has no signal to know when to fetch the file."
---

# Test 08 — description frontmatter

## Technique

> Every agent-readable file gets a `description:` frontmatter field.
> **When:** Every file that may be stub-loaded by pi-context. Without it, the LLM has no signal to know when to fetch the file.

## Why this test exists

A `description:` field is how a loading agent decides whether to fetch the full file. If it's missing or vague, the agent can't route correctly. The skill must always write a tight one-sentence `description:` in YAML frontmatter that states the trigger condition — not a title or a summary of contents.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a caching rules document for our Redis layer.

Rules:
- Set a TTL on every key — never cache without expiry
- Use the pattern service:entity:id for all cache keys
- Never cache user-specific data longer than 15 minutes
- Invalidate the cache on every write, not just on TTL expiry
- Never store raw database rows — always serialize to a public DTO
```

## What to observe

The skill must:
- Include a YAML frontmatter block at the top of the file
- The `description:` field must be a trigger-condition sentence — when should an agent load this file?
- Not use the document title as the description
- Not write a summary of contents as the description

| What to check | Good | Bad |
|---|---|---|
| Frontmatter present | `---\ndescription: ...\n---` at line 1 | No frontmatter at all |
| Trigger-condition phrasing | "Load when writing or reviewing Redis caching logic" | "Redis caching rules document" |
| Not a title | Starts with "Load when…" or "Use when…" | Starts with "This document covers…" |
| Specific enough | Names the domain ("Redis", "caching", "cache keys") | Generic ("Load when working on this project") |

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Frontmatter present | File starts with `---` and contains a `description:` field before `---` closes |
| C2 | Trigger-condition phrasing | `description:` starts with "Load when", "Use when", or similar action-oriented phrase |
| C3 | Domain-specific | `description:` names the relevant domain (caching, Redis, cache keys, or equivalent) |
| C4 | Not a title | `description:` is not the document title ("Redis Caching Rules") or a table of contents |
| C5 | One sentence | `description:` is a single sentence, not a paragraph |

## Fail signals

- No YAML frontmatter block at all
- `description:` is the document title verbatim
- `description:` is a paragraph describing all sections
- `description:` is so generic it could apply to any document ("Load when working on backend code")
