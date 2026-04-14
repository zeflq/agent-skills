---
technique: "Tables for mappings"
when: "Exactly 2 flat columns, all rows consumed together for understanding, no future fields expected. If the LLM acts on individual rows rather than reading the whole table, use XML instead."
---

# Test 02 — Tables for mappings

## Technique

> Use markdown tables for X → Y content (concept → definition, status → meaning, input → expected output).
> **When:** Exactly 2 flat columns, all rows consumed together for understanding, no future fields expected. If the LLM acts on individual rows rather than reading the whole table, use XML instead.

## Why this test exists

This technique has a sharp boundary: 2-column reference data read all-at-once → table. Individual rows the agent acts on one at a time → XML. The input below contains both kinds of sections, so the skill must apply each format correctly — using tables where appropriate and switching to XML where the agent acts on individual rows.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a deployment pipeline reference document.

Glossary of deployment terms:
- artifact: the versioned build output uploaded to the registry
- canary: a partial rollout to a small percentage of traffic
- rollback: reverting to the previous stable artifact
- smoke test: a fast post-deploy check that the service is responding

Deployment status meanings:
- pending: deployment queued, no action needed
- running: deployment in progress, do not interrupt
- failed: deployment failed, trigger rollback
- success: deployment complete, run smoke tests

Health checks:
- name: api-root, endpoint: GET /, expected status: 200, action on failure: restart api service
- name: db-ping, endpoint: GET /healthz/db, expected status: 200, action on failure: alert on-call
- name: cache-ping, endpoint: GET /healthz/cache, expected status: 200, action on failure: restart cache service
```

## What to observe

The input has three sections with different structures:

| Section | Columns | Consumed how | Expected format |
|---|---|---|---|
| Glossary terms → definitions | 2 (term → definition) | All at once to understand vocabulary | Table |
| Status → meaning | 2 (status → meaning) | All at once as reference | Table |
| Health checks | 4 (name + endpoint + expected + action) | One at a time by the agent | XML |

The skill must distinguish: first two sections are flat 2-column lookups read all at once → table. Third section has 4 fields and the agent acts on each check individually → XML.

## Checks

Run the evaluator with the generated document and verify:

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Glossary uses XML wrapper + markdown table | Named XML section tag (e.g. `<glossary>`) wraps a 2-column markdown table — no XML child tags for individual rows |
| C2 | Deployment statuses use XML wrapper + markdown table | Named XML section tag wraps a 2-column markdown table — no XML child tags for individual rows |
| C3 | Health checks use XML | Each health check in an XML block with named child tags (endpoint, expected status, action) — not a table |
| C4 | No XML child tags for table rows | Rows inside table sections are not wrapped in individual XML tags — table rows only |

## Fail signals

- Table sections missing their XML section wrapper (section has no named boundary)
- Individual table rows wrapped in XML child tags instead of markdown rows
- Health checks written as a table (loses the 4th field and collapses per-item structure)
- Any section uses bullet lists instead of the correct format
