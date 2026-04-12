# Discovery Interview — agent-doc

Use these questions to discover what document to build and what sections it needs.
Ask in order. Stop as soon as you have enough to propose a section structure.

## Step 1 — Three Discovery Questions

Ask all three in one message (exception to the one-at-a-time rule — these are scoping questions):

1. **What is this document for?** — What does it describe or govern?
2. **Who reads it?** — Which agent, in which context, will load this file?
3. **What decisions does it guide?** — What should the agent know or do differently after reading it?

## Step 2 — Propose a Section Structure

Based on the answers, propose a section list. Use the mapping below as a starting point.

| Document type | Suggested sections |
|---|---|
| Architecture / tech guide | Overview, Stack & Constraints, Component Map, Patterns, Anti-patterns |
| Runbook / workflow | Purpose, Prerequisites, Steps, Verification, Rollback |
| API contract | Purpose, Authentication, Endpoints, Request/Response Format, Error Handling, Rate Limits |
| Style / coding guide | Purpose, Naming Conventions, File Structure, Patterns, Forbidden Patterns |
| Schema / data guide | Purpose, Entities, Relationships, Key Fields, Query Patterns, Off-limits Operations |
| Deployment guide | Purpose, Environments, Steps, Required Checks, Rollback |
| Generic rules doc | Purpose, Rules, Exceptions, Verification |

Propose the list, then ask: *"Does this match what you need? Any sections to add or remove?"*

## Step 3 — Interview Per Section

For each confirmed section:
- Show a concrete XML example
- Ask the user to adapt it or provide their own content
- Apply all techniques from `references/techniques.md`

## Step 4 — description Frontmatter

Always ask last: *"In one sentence, when should an agent load this file?"*
Use the answer as the `description:` field.

**Example:**
```markdown
---
description: Read this before touching any frontend component — contains naming conventions, allowed patterns, and anti-patterns for the UI layer.
---
```
