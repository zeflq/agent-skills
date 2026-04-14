---
description: Ordered interview workflow for discovering document scope, sections, and section-level writing techniques before drafting.
---

# Discovery Interview — agent-doc

<workflow>
  <step number="1" name="Ask discovery questions">
    <action>Ask all four questions in one message.</action>
    <action>Capture answers as draft inputs for scope, audience, decisions, and load trigger.</action>
    <example>
      1. What is this document for?
      2. Who reads it, and in what context?
      3. What decisions should the reader make differently after reading it?
      4. In one sentence, when should an agent load this file?
    </example>
  </step>

  <step number="2" name="Decide if scope is sufficient">
    <if>IF scope, audience, and decision intent are clear, continue to Step 3.</if>
    <else>ELSE ask only the minimum follow-up questions required to remove ambiguity, then continue to Step 3.</else>
    <rule>Never ask for information already provided by the user.</rule>
  </step>

  <step number="3" name="Propose section structure">
    <action>Propose sections and assign one writing technique per section from references/techniques.md.</action>
    <action>Ask the user to confirm, add, or remove sections before drafting content.</action>
    <example>"Proposed sections: steps (strict sequential workflow), rules (XML tags), verification (self-verification loop). Keep or edit?"</example>
  </step>

  <step number="4" name="Interview section by section">
    <action>Handle one confirmed section at a time.</action>
    <action>Show one filled example in the selected technique format, then ask the user to adapt or replace it.</action>
    <rule>Never draft a later section before the current section is confirmed.</rule>
  </step>
</workflow>

<section-template-map>

| Document type | Suggested sections (XML tag names) |
|---|---|
| Architecture / tech guide | `<overview>`, `<stack>`, `<patterns>`, `<anti-patterns>`, `<verification>` |
| Runbook / workflow | `<prerequisites>`, `<steps>`, `<verification>`, `<rollback>` |
| API contract | `<authentication>`, `<endpoints>`, `<error-handling>`, `<rate-limits>` |
| Style / coding guide | `<naming>`, `<file-structure>`, `<patterns>`, `<forbidden>` |
| Schema / data guide | `<entities>`, `<allowed-operations>`, `<query-patterns>`, `<off-limits>` |
| Deployment guide | `<environments>`, `<steps>`, `<verification>`, `<rollback>` |
| Generic rules doc | `<allowed-scope>`, `<rules>`, `<exceptions>`, `<verification>` |

</section-template-map>

<constraint-ordering>
  <rule>Apply constraints before guidelines inside every section.</rule>
  <rule>Place hard constraints first: security, correctness, data integrity, absolute prohibitions.</rule>
  <rule>Place soft guidelines after hard constraints: style, convention, performance preferences.</rule>
  <rule>Never interleave hard and soft rules.</rule>
  <example>
    <rules>
      <rule><requirement>Never return HTTP 200 with an error body.</requirement></rule>
      <rule><requirement>Never omit request_id in any response.</requirement></rule>
      <rule><requirement>Use camelCase for JSON keys.</requirement></rule>
      <rule><requirement>Keep response payloads under 1MB.</requirement></rule>
    </rules>
  </example>
</constraint-ordering>

<self-check>
  <check>All four discovery questions were asked before section drafting.</check>
  <check>Each proposed section has exactly one selected technique.</check>
  <check>Every drafted section includes a concrete example.</check>
  <check>Hard constraints appear before soft guidelines in mixed sections.</check>
</self-check>
