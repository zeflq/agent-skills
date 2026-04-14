---
technique: "Strict sequential workflow"
when: "Any multi-step process where sequence matters. If steps are unordered, use a list instead."
---

# Test 05 — Strict sequential workflow

## Technique

> Workflow steps are numbered. Order is enforced.
> **When:** Any multi-step process where sequence matters. If steps are unordered, use a list instead.

## Why this test exists

Users describe multi-step processes in prose or unordered form. The skill must recognize when order matters and produce numbered `<step>` elements inside a `<steps>` block. The input below is a sequence that cannot be reordered — the skill must not use a bullet list.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write a deployment runbook for our Python service.

Deployment steps:
- Run the test suite and confirm it passes
- Build and push the Docker image with the new tag
- Update the Helm values file with the new image tag
- Open a PR for the Helm values change and get it approved
- Merge the PR and wait for ArgoCD to sync
- Verify the new pods are Running and Ready
- Smoke-test the /health endpoint
- If smoke test fails, roll back by reverting the Helm values PR
```

## What to observe

Steps are strictly ordered — running smoke tests before deployment or syncing before merging would break the process. The skill must:
- Produce a `<steps>` block with numbered `<step>` elements
- Preserve the original order exactly
- Not flatten into a bullet list

| Step | Why order matters |
|---|---|
| 1. Run tests | Must pass before any artifact is built |
| 2. Build image | Must exist before Helm values can reference it |
| 3. Update Helm values | Must reference the new image before PR can be opened |
| 4. PR + approval | Must be approved before merging |
| 5. Merge + sync | Must happen before pods update |
| 6. Verify pods | Must be running before smoke test |
| 7. Smoke test | Final gate before declaring success |
| 8. Rollback on failure | Conditional — only if step 7 fails |

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | `<steps>` wrapper present | Output contains a `<steps>` XML block |
| C2 | Steps are numbered | Each step is a `<step>` element with a `number` attribute or sequential position |
| C3 | All 8 steps present | No step dropped or merged |
| C4 | Order preserved | Steps appear in the same order as the input |
| C5 | No bullet list | Steps are not rendered as an unordered markdown list |
| C6 | Rollback is conditional | Step 8 is framed as a conditional (on failure), not an unconditional step |

## Fail signals

- Steps rendered as a bullet list instead of numbered `<step>` elements
- Steps reordered (e.g., smoke test moved before pod verification)
- Rollback step presented as unconditional
- Any step dropped or silently merged with another
