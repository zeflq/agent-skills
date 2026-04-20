---
technique: "Multi-file document system"
when: "When building a folder of 3+ related agent docs — each doc owns one domain, a root index doc links them, rules are never copied across docs."
---

# Test 12 — Multi-file document system

## Technique

> Each doc owns exactly one domain. Never copy rules across docs — link to the doc that owns them. Create one root index doc when the system has 3+ docs.
> **When:** When a user needs a system of related agent docs covering distinct domains.

## Why this test exists

When a user asks for multiple related docs, the skill must assign each topic to exactly one doc and produce a root index that links them — not a single monolithic file or a set of files that repeat each other's rules.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
I need a set of agent docs for my platform. The agent will read them as context when working on different tasks.

Topics to cover:
- Authentication: how to get and refresh tokens, Bearer auth headers, token expiry (1h), API key fallback (read-only, never expires)
- Database: allowed queries, forbidden operations (never DROP, never raw DELETE without WHERE), connection pooling rules, migration steps
- API usage: endpoint list, rate limits, error handling, retry policy

The auth rules should also appear in the API doc since the agent calls APIs with auth headers.
```

## What to observe

The input explicitly asks for auth rules in two places (auth doc + API doc). The skill must:
1. Assign auth rules to `auth.md` only
2. Have `api.md` reference `auth.md` — not copy the rules
3. Produce a root index doc linking all three
4. Never duplicate rules across files

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Three domain docs produced | Separate files for auth, database, and API — each with `description:` frontmatter |
| C2 | Root index doc produced | A fourth file that describes the system and links to all three domain docs — owns no domain rules itself |
| C3 | Auth rules live in one doc | Auth rules (token format, expiry, API key) appear only in `auth.md` — not copied into `api.md` |
| C4 | API doc references auth doc | `api.md` links to `auth.md` for auth rules instead of inlining them |
| C5 | No cross-doc duplication | No rule or content block appears verbatim in more than one file |

## Fail signals

- Auth rules copied into both `auth.md` and `api.md`
- No root index doc produced
- All content collapsed into a single file
- A doc is missing a `description:` frontmatter field
- Root index doc contains domain rules instead of only links
