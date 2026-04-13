# Discovery Interview — agent-doc

Use these questions to discover what document to build and what sections it needs.
Ask in order. Stop as soon as you have enough to propose a section structure.

## Step 1 — Four Discovery Questions

Ask all four in one message (exception to the one-at-a-time rule — these are scoping questions):

1. **What is this document for?** — What does it describe or govern?
2. **Who reads it?** — Who or what will load this file, and in what context?
3. **What decisions does it guide?** — What should the reader know or do differently after reading it?
4. **In one sentence, when should an agent load this file?** — This becomes the `description:` frontmatter.

## Step 2 — Propose a Section Structure

Based on the answers, propose a section list with the technique each section will use. Use the mapping below as a starting point.

| Document type | Suggested sections (XML tag names) |
|---|---|
| Architecture / tech guide | `<overview>`, `<stack>`, `<patterns>`, `<anti-patterns>`, `<verification>` |
| Runbook / workflow | `<prerequisites>`, `<steps>`, `<verification>`, `<rollback>` |
| API contract | `<authentication>`, `<endpoints>`, `<error-handling>`, `<rate-limits>` |
| Style / coding guide | `<naming>`, `<file-structure>`, `<patterns>`, `<forbidden>` |
| Schema / data guide | `<entities>`, `<allowed-operations>`, `<query-patterns>`, `<off-limits>` |
| Deployment guide | `<environments>`, `<steps>`, `<verification>`, `<rollback>` |
| Generic rules doc | `<allowed-scope>`, `<rules>`, `<exceptions>`, `<verification>` |

For each proposed section, select the technique based on its `<when>` condition in `references/techniques.md`.

Example proposal:
> *"Here's the structure I'd suggest:*
> - **steps** — strict sequential workflow → numbered `<step>` elements inside `<steps>`
> - **error-handling** — table for mappings → markdown table (status → action), no XML wrapper
> - **rules** — XML tags → `<rule>` elements inside `<rules>`
>
> Does this match what you need? Any sections to add or remove?"*

## Step 3 — Interview Per Section

For each confirmed section:
- Use the technique selected for it in Step 2
- Show a concrete example using that technique
- Ask the user to adapt it or provide their own content

## Constraints — enforce throughout the interview

Apply the constraints defined in `references/techniques.md` as content grows — do not wait until the end.
