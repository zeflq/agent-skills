# agent-doc Base Template

Use this as the base for any agent-readable document.
Replace [placeholders], add discovered sections, apply techniques throughout.

---

```markdown
---
description: [One sentence — when should an agent load this file?]
---

# [Document Title]

<!-- ============================================================
     Sections below are wrapped in XML tags for clear boundaries.
     Each section name reflects its purpose.
     ============================================================ -->

<[section-name]>
  [Content — imperative language, explicit rules, XML structure]
</[section-name]>

---

<[section-name]>
  [Content]
</[section-name]>
```

---

## Example — Frontend Architecture Doc

```markdown
---
description: Read before touching any frontend component — naming conventions, allowed patterns, and anti-patterns for the UI layer.
---

# Frontend Architecture

<overview>
  <purpose>Next.js App Router frontend — product catalog and checkout UI</purpose>
  <owner>Frontend team — changes require PR review</owner>
</overview>

---

<stack>
  <framework>Next.js 14 App Router — server components by default</framework>
  <styling>Tailwind CSS only — no CSS modules, no inline styles</styling>
  <state>Zustand for client state — no Redux, no Context for global state</state>
  <data-fetching>Server components fetch directly — client components use SWR</data-fetching>
</stack>

---

<patterns>
  <rule>Co-locate component, test, and styles in the same folder</rule>
  <rule>Named exports only — no default exports</rule>
  <rule>Server component unless interactivity requires client — add "use client" last resort</rule>
  <rule>Reuse existing components before creating new ones — check /components first</rule>
</patterns>

---

<anti-patterns>
  <never>useEffect for data fetching — use server components or SWR</never>
  <never>Direct DOM manipulation — use React state</never>
  <never>Hardcode colors or spacing — use Tailwind tokens only</never>
</anti-patterns>

---

<verification>
  <check>Component renders without console errors</check>
  <check>pnpm test --run passes</check>
  <check>No new dependencies added without approval</check>
</verification>
```
