---
technique: "XML tags for structure"
when: "Any file with 3+ fields per item, or when the LLM acts on individual items (reads, calls, fetches)"
---

# Test 01 — XML tags for structure

## Technique

> Wrap every section in a named XML block.
> **When:** Any file with 3+ fields per item, or when the LLM acts on individual items (reads, calls, fetches). Use even for 2-field items when the LLM selects and acts on them one at a time.

## Why this test exists

The skill should use XML blocks — not markdown lists or prose — whenever a section contains items with 3+ fields. This test drives an API contract document where every item (endpoint, error code, auth method) has 3+ fields, leaving no ambiguity: XML is the only correct choice.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an API contract document for my REST API. The agent reading this will call endpoints one at a time based on the task.

Endpoints:
- GET /users/{id}: returns a user object, requires Bearer auth, rate-limited to 100/min
- POST /users: creates a user, requires Bearer auth, accepts JSON body with email+password, rate-limited to 10/min
- DELETE /users/{id}: deletes a user, requires Bearer auth + admin role, rate-limited to 5/min

Error codes the agent must handle:
- 401: token missing or expired — refresh token and retry once
- 403: insufficient role — stop and report to user
- 429: rate limit hit — wait 60s and retry
- 500: server error — retry once, then report

Auth methods supported:
- Bearer token: header Authorization: Bearer <token>, expires in 1h
- API key: header X-API-Key: <key>, never expires, read-only scope only
```

## What to observe

Every item in the input has 3+ fields:
- Each endpoint: method + path + auth + rate-limit (+ body for POST) — 4–5 fields
- Each error code: code + meaning + required agent action — 3 fields
- Each auth method: name + header format + expiry + scope — 4 fields

The LLM will act on individual items (call one endpoint at a time, handle one error at a time), so the `<when>` condition is fully met for all sections.

## Checks

Run the evaluator with the generated document and verify:

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Endpoints use XML | Each endpoint in a named XML block with child tags (e.g., `<method>`, `<auth>`, `<rate-limit>`) — not a markdown list |
| C2 | Error codes use XML | Each error in a named XML block with child tags for code, meaning, and action — not a table or list |
| C3 | Auth methods use XML | Auth section wrapped in a named XML block (`<authentication>` or equivalent) with named child tags — not a markdown list. Internal structure (per-method blocks vs rules) is a valid design choice. |
| C4 | No unnamed `<item>` tags | Tags are semantically named (`<endpoint>`, `<error>`, `<auth-method>`) not generic `<item>` repeated |
| C5 | No markdown lists where XML required | No `- bullet` lists inside sections where items have 3+ fields |

## Fail signals

- Any section uses bullet lists (`- item`) instead of XML where `<when>` applies
- XML present but tags are unnamed or generic (`<item>` × N)
- Sections written as prose paragraphs
- Error codes written as a table (2-column would be acceptable only if action wasn't a required third field)
