---
technique: "Token budget"
when: "Always — root doc must not exceed 80–120 lines. Apply cut/keep rules before writing."
---

# Test 13 — Token budget

## Technique

> Root agent doc must not exceed 80–120 lines. Cut: explanatory prose, redundant examples, speculative rules, empty XML blocks. Keep: hard constraints, workflow steps, verification checklist, permissions model.
> **When:** Always — apply before writing any document, regardless of how much content the user provides.

## Why this test exists

Users often provide more content than a root doc should contain — long explanations, multiple examples per rule, speculative edge cases. The skill must apply the cut/keep rules and stay within budget rather than faithfully reproducing everything.

## Input

Paste this as the user prompt when running the agent-doc skill:

```
Write an architecture doc for my frontend. The agent will read this when working on UI tasks.

Stack:
- React 18 with TypeScript
- Tailwind CSS for styling
- React Query for server state
- Zustand for client state
- Vite as the bundler

Component rules:
- Never use class components — we migrated to hooks in 2021 after a long debate about maintainability and the team collectively decided hooks were cleaner, more composable, and easier to test. Every component should be a function component.
- Co-locate styles with components using Tailwind utility classes. We tried CSS modules before and found them hard to maintain across the team because people kept naming things differently.
- Never use inline styles. The reason is that inline styles bypass Tailwind's purging and can cause inconsistencies in production builds.
- Export one component per file. This helps with tree shaking and makes imports predictable. We debated this for a while but settled on it after seeing import confusion in larger files.
- Never import from a sibling's internal files — only from its index. This was a lesson learned after a painful refactor in Q3 2023 where internal imports caused circular dependency issues.

State rules:
- Use React Query for all server state — fetching, caching, invalidation. Do not use useEffect + useState for data fetching. This is because React Query handles loading/error states, background refetching, and cache invalidation automatically. We wasted weeks reinventing these behaviors before adopting it.
- Use Zustand for global client state only. Do not use it for server state. Keep stores small and focused — one store per domain. We've tried Redux and Context API before; Zustand was simpler and caused fewer re-renders.
- Never put derived state into a store — compute it in the component or with a selector. Derived state in stores caused stale bugs in our dashboard last year.

Error handling:
- Wrap every page-level component in an ErrorBoundary. This is a hard requirement from the team after a production incident in January where an uncaught render error took down the whole app.
- Use React Query's onError callback for API errors — never swallow them silently.
- Log errors to Sentry. We evaluated Datadog and LogRocket but chose Sentry for its React integration and affordable pricing.
- Never show raw error messages to users — map to user-friendly strings.

Testing:
- Unit test every custom hook with React Testing Library.
- Integration test page-level components.
- Never mock React Query in tests — use MSW to intercept requests instead. We got burned by mocked tests passing while real API calls failed.
- Aim for 80% coverage on business logic. We know 100% is unrealistic and causes test brittleness.
- Run tests in CI before every merge.

Accessibility:
- Every interactive element must have an aria-label if it lacks visible text.
- Use semantic HTML elements — nav, main, section, article — before reaching for div.
- Colour contrast must meet WCAG AA standards. Our design system enforces this but custom components need manual checks.
- Tab order must follow the visual order of the page.
```

## What to observe

The input is deliberately over-long — every rule has a lengthy explanation of its history or rationale. The skill must:
- Strip the "why we decided this" prose from every rule
- Keep the hard constraints and soft preferences
- Produce a doc under 120 lines
- Retain all distinct rules — only the explanatory prose is cut

## Checks

| # | Check | Pass condition |
|---|-------|----------------|
| C1 | Output under 120 lines | Total line count of the written file does not exceed 120 |
| C2 | Explanatory prose removed | No "we tried X before", "after a debate", "because of an incident" backstory in any rule |
| C3 | All hard constraints present | Every "never" rule from the input appears in the output |
| C4 | Soft preferences present | Style and convention rules retained — only prose rationale cut, not the rules themselves |
| C5 | No empty XML blocks | No `<section></section>` or similar placeholder blocks |

## Fail signals

- Output exceeds 120 lines
- Rules retain multi-sentence historical explanations ("we migrated in 2021 after...", "a lesson learned after...")
- Any hard constraint ("never use class components", "never mock React Query") is dropped to save space
- Rules are truncated rather than having their prose rationale stripped
