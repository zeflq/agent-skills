---
description: Canonical techniques for writing agent-readable documents — format, structure, and constraint rules.
---

# Agent Document Writing Techniques

These techniques apply to any file written for an agent to read.
They are the source of truth — copied into each skill's references/ for self-contained packaging.

<techniques>
  <technique>
    <name>XML tags for structure</name>
    <application>Wrap every section in a named XML block. Ensures clear boundaries and reliable LLM parsing.</application>
    <when>Any file with 3+ fields per item, or when the LLM acts on individual items (reads, calls, fetches). Use even for 2-field items when the LLM selects and acts on them one at a time.</when>
  </technique>
  <technique>
    <name>Tables for mappings</name>
    <application>Use markdown tables for X → Y content (concept → definition, status → meaning, input → expected output).</application>
    <when>Exactly 2 flat columns, all rows consumed together for understanding, no future fields expected. If the LLM acts on individual rows rather than reading the whole table, use XML instead.</when>
  </technique>
  <technique>
    <name>Imperative language</name>
    <application>Rewrite any soft language. "prefer not to" → "never". "try to" → "always".</application>
    <when>Every rule or constraint in any agent-readable file. Never leave soft language in place.</when>
  </technique>
  <technique>
    <name>Constraints before guidelines</name>
    <application>Within any section, hard rules appear before soft preferences.</application>
    <when>Every section that mixes rules and preferences. Order is enforced — never interleave them.</when>
  </technique>
  <technique>
    <name>Strict sequential workflow</name>
    <application>Workflow steps are numbered. Order is enforced.</application>
    <when>Any multi-step process where sequence matters. If steps are unordered, use a list instead.</when>
  </technique>
  <technique>
    <name>Self-verification loop</name>
    <application>Checklist forces the agent to verify its own work before responding.</application>
    <when>Any workflow that produces output the agent could get wrong silently. Place at the end of the workflow.</when>
  </technique>
  <technique>
    <name>Examples not explanations</name>
    <application>Each section shows a concrete filled-in example. No prose explanations inside the doc.</application>
    <when>Every rule or technique that could be misapplied. One example beats three sentences of explanation.</when>
  </technique>
  <technique>
    <name>description frontmatter</name>
    <application>Every agent-readable file gets a `description:` frontmatter field.</application>
    <when>Every file that may be stub-loaded by pi-context. Without it, the LLM has no signal to know when to fetch the file.</when>
  </technique>
  <technique>
    <name>Trigger word tables</name>
    <application>Map keywords or phrases the user might say to a specific action the agent must take. Format: 2-column table, left = trigger pattern, right = required action.</application>
    <when>Any section that routes agent behavior based on user input. Use when the same word in different contexts must always trigger the same action. Covers synonyms — "delete", "remove", "drop" are one row.</when>
  </technique>
  <technique>
    <name>If-then logic</name>
    <application>Write conditional rules as explicit IF / ELSE IF branches with numbered steps. Never use prose for decisions. Each branch ends with a concrete action, never "use judgment".</application>
    <when>Any decision the agent must make consistently. If a branch exceeds 4 steps, move it to a linked file and reference it from the branch.</when>
  </technique>
</techniques>

<constraints>
  <constraint>
    <name>Section length</name>
    <rule>A section must not exceed 15 lines. If it does, ask the user what to cut before writing more.</rule>
  </constraint>
  <constraint>
    <name>File budget</name>
    <rule>A root agent file must not exceed 80–120 lines. Total across all loaded files must not exceed 300 lines.</rule>
  </constraint>
  <constraint>
    <name>Split into linked files</name>
    <rule>Split a section into a linked file when: it exceeds 20 lines, applies to fewer than 30% of tasks, or changes at a different rate than the rest of the file. Every linked file must have a `description:` frontmatter field.</rule>
  </constraint>
  <constraint>
    <name>Allowlist over denylist</name>
    <rule>Any section that grants permissions must use an explicit allowed list. Anything not listed is off-limits by default. Never use a deny list as the primary access control.</rule>
  </constraint>
  <constraint>
    <name>No duplication</name>
    <rule>Every rule or content block must live in exactly one place. If the same content appears in both a root file and a linked file, remove it from one. Never copy rules across files — reference the file that owns them instead. If user-provided content contains duplicate rules or repeated blocks, consolidate before writing — never preserve duplication from the input.</rule>
  </constraint>
</constraints>
