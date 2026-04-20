---
description: Evaluator prompt for testing agent-doc technique application — load with a scenario file and generated document output to score.
---

# agent-doc Technique Evaluator

## How to use

1. Load the scenario file: `tests/scenarios/NN-technique-name.md`
2. Run the agent-doc skill with the scenario's **Input** section as the user prompt
3. Paste the generated document here as **Output**
4. Run this evaluator prompt to get a pass/fail score per technique check

---

## Evaluation prompt

```
You are a strict evaluator for the agent-doc skill.

You will be given:
- TECHNIQUE: the technique being tested (name + when condition)
- INPUT: what was given to the agent-doc skill
- OUTPUT: the document the skill produced
- CHECKS: the specific assertions to verify

For each CHECK, answer:
  PASS — the output satisfies the check
  FAIL — the output does not satisfy the check, explain why in one line
  N/A  — the check does not apply to this output

Then give an overall verdict: PASS (all checks pass) or FAIL (any check fails).

Be strict. Partial compliance counts as FAIL.
```

---

## Score table (fill in after each run)

| # | Technique | Status | Notes |
|---|-----------|--------|-------|
| 01 | XML tags for structure | PASS | |
| 02 | Tables for mappings | PASS | |
| 03 | Imperative language | PASS | |
| 04 | Constraints before guidelines | PASS | Fixed: rules were split across 3 sections; consolidated into one `<rules>` block, hard constraints first |
| 05 | Strict sequential workflow | PASS | |
| 06 | Self-verification loop | PASS | |
| 07 | Examples not explanations | PASS | |
| 08 | description frontmatter | PASS | |
| 09 | Trigger word tables | PASS | |
| 10 | If-then logic | PASS | |
| 11 | No duplication | PASS | Multiple iterations — final fix: pre-write classification rules + blocking verification gate |
| 12 | Constraints before guidelines — any section name | PASS | |
| 13 | Strict sequential workflow — any section name | PASS | |
| 14 | Constraints before guidelines — any section name v2 | PASS | |
| 15 | Strict sequential workflow — any section name v2 | PASS | |
| 16 | Multi-file document system | PASS | |
| 17 | Token budget | PASS | Separate issue: constraints-before-guidelines failed in `<testing>` section — hard rules at positions 3-4 after soft rules |
