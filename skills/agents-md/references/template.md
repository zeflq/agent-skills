# AGENTS.md Template

Fill sections 1–7 (required) and 8–10 (recommended) with user answers.
Use XML tags for all sections. Apply techniques from references/sections.md.

---

```markdown
# ============================================================
# AGENTS.md — [Project Name]
# ============================================================

<scope>
  <purpose>[What the project does]</purpose>
  <stack>
    <language>[Language and version]</language>
    <framework>[Framework]</framework>
    <package-manager>[pnpm / npm / yarn / uv / etc.]</package-manager>
  </stack>
  <focus>[What areas the agent works on — what it does NOT touch]</focus>
</scope>

---

<permissions>
  <allowed>
    <item>[Path or action the agent can do freely]</item>
  </allowed>
  <ask-first>
    <item>[Action that requires user confirmation]</item>
  </ask-first>
  <never>
    <item>[Absolute off-limits — no exceptions]</item>
  </never>
</permissions>

---

<constraints>
  <never>[Absolute project-level rule]</never>
  <never>[Absolute project-level rule]</never>
</constraints>

---

<standards>
  <naming>
    <[type]>[Convention]</[type]>
  </naming>
  <rules>
    <rule>[Specific coding rule — imperative]</rule>
  </rules>
</standards>

---

<commands>
  <test>[exact test command with flags]</test>
  <lint>[exact lint command with flags]</lint>
  <typecheck>[exact typecheck command]</typecheck>
  <format>[exact format command]</format>
  <build>[exact build command]</build>
</commands>

---

<workflow>
  <step order="1">Analyze — understand the full request before touching any file</step>
  <step order="2">Locate — find all relevant files using search</step>
  <step order="3">Plan — write a one-paragraph plan; stop if scope exceeds 5 files</step>
  <step order="4">Edit — make the smallest change that achieves the goal</step>
  <step order="5">Verify — run lint, typecheck, and tests before responding</step>
  <rule>If plan reveals unexpected scope → stop and report before proceeding</rule>
  <rule>If a command fails twice → report it, do not retry blindly</rule>
  <rule>If uncertain → ask one clarifying question, then stop</rule>
</workflow>

---

<verification>
  <check>[Test command] passes with no failures</check>
  <check>[Lint command] reports zero warnings</check>
  <check>No files outside allowed scope were modified</check>
  <check>No new dependencies added without approval</check>
  <check>Output matches original requirement</check>
</verification>

---

<!-- RECOMMENDED — Output Expectations -->
<!--
<output>
  <mode>implement-directly</mode>
  <format>[diff / full file / explanation only]</format>
  <explanation>[none / brief / detailed]</explanation>
</output>
-->

---

<!-- RECOMMENDED — Safety Rules -->
<!--
<safety>
  <never>Output secrets, tokens, or credentials in any form</never>
  <never>Hardcode sensitive values — use environment variables</never>
  <confirm-before>
    <action>[Irreversible action]</action>
  </confirm-before>
</safety>
-->

---

<!-- OPTIONAL — Agent Roles (only for multi-agent projects) -->
<!--
<roles>
  <role name="[role-name]">
    <scope>[Allowed paths and responsibilities]</scope>
    <never>[What this role must not touch]</never>
  </role>
</roles>
-->
```
