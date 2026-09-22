Summary
-------
Project Pulse is a small, learner-friendly dashboard that displays the current status of projects from a local JSON seed file. This plan describes precise, phased work to create a deterministic, testable implementation using app/index.html, app/styles.css, app/project-data.json, and .vscode/launch.json. The plan separates Designer and Coder responsibilities, identifies file-level ownerships to avoid merge conflicts, and lists validation steps, dependencies, and edge cases.

Phases (ordered, numbered)
--------------------------
Phase 1  Project scaffolding and kickoff (quick, prerequisite)
1. Create app/ directory and add placeholder files:
   - app/index.html (shell)
   - app/styles.css (empty)
   - app/project-data.json (seed file)
   - app/main.js (minimal loader)  optional but recommended for separation
   - .vscode/launch.json (workspace debug/preview config)
2. Add docs/project-pulse-plan.md (this plan) to docs/.

Phase 2  Data model and seed data (Coder)
3. Design and implement the JSON schema for projects in app/project-data.json.
4. Populate with 68 realistic seed entries for testing: variety of statuses, progress values, long text, missing optional fields.

Phase 3  Visual system & CSS hooks (Designer)
5. Designer produces styles and design tokens in app/styles.css:
   - Color variables, type scale, spacing scale, breakpoints
   - Component hooks: .dashboard, .project-card, .project-list, .project-header, .project-meta, .progress, .status-pill
   - Accessibility rules (focus outlines, contrast guidance)
6. Produce a one-page design-spec document (docs/project-pulse-design.md) describing responsive behavior and accessibility expectations.

Phase 4  Static HTML & accessibility-first markup (Coder)
7. Implement app/index.html:
   - Semantic structure and placeholders that map to design hooks (header, main.dashboard, section.project-list)
   - Minimal inline ARIA where applicable (role="status", aria-live for updates)
   - Link to app/styles.css and app/main.js
8. Implement app/main.js to fetch app/project-data.json, render project cards into .project-list, and handle basic error states.

Phase 5  Interaction & progressive enhancement (Coder)
9. Add client behaviors:
   - Filter by status (All, Active, Blocked, Completed)
   - Search by name/owner/tags
   - Sort by progress/lastUpdated
   - Keyboard-accessible controls with ARIA labels
10. Error handling and loading states:
   - Show a spinner or "Loading" while fetching
   - Show a friendly error banner if fetch fails or JSON is invalid
   - Fallback UI for empty data set

Phase 6  Polishing, responsive testing, and VS Code preview config (Designer + Coder)
11. Run accessibility checks and responsive tests:
   - Colors meet AA contrast
   - Keyboard navigation works
   - Layout collapses correctly at mobile breakpoints
12. Final edits to .vscode/launch.json so the run/debug configuration opens the app root and index.html in browser preview.

Phase 7  Final verification and docs (Coder + Designer)
13. Add README snippet or docs/README-pulse.md explaining how to run preview (Live Server or launch.json), and how to update seed data.
14. Final cross-check: complete final-checklist.

File assignments (explicit)
--------------------------
- docs/project-pulse-plan.md
  - Owner: Planner (this deliverable). Already produced by this plan  Orchestrator will save it.

- app/index.html
  - Owner: Coder
  - Scope: semantic markup, links to styles and main.js, container markup (header, search/filter controls, main.dashboard, .project-list placeholder).
  - Deterministic behavior: no network-only resources; use relative links.

- app/styles.css
  - Owner: Designer
  - Scope: visual style tokens, layout, responsive rules, and CSS classes named as hooks (.dashboard, .project-card, .project-list, .project-meta, .progress, .status-pill).
  - Must export variables for colors and spacing to be re-used in inline-progress bars or small JS-driven style changes.

- app/project-data.json
  - Owner: Coder (prepare seed and update as needed)
  - Scope: static seed data file consumed by fetch in app/main.js. See sample schema below.

- app/main.js
  - Owner: Coder
  - Scope: fetch project-data.json, render DOM, wire up interactions (filter/search/sort), handle errors and empty states. Keep code small and well-commented.

- .vscode/launch.json
  - Owner: Coder
  - Scope: set "cwd" to ${workspaceFolder}/app and provide a launch configuration that opens index.html in the browser preview (example provided in Validation section).

Designer responsibilities
------------------------
- Produce app/styles.css according to design tokens and accessibility rules.
- Provide CSS hooks exactly as named: .dashboard, .project-card, .project-list, .project-header, .project-meta, .progress, .status-pill, .control-bar, .search-input.
- Provide a short design-spec file at docs/project-pulse-design.md with:
  - Color palette (variables)
  - Type scale
  - Responsive breakpoints and how the project cards should rearrange
  - Focus and hover states
  - Accessibility notes (contrast ratios, ARIA patterns to use)
- Provide small example screenshots or ASCII diagrams if necessary.
- Keep stylesheet deterministic: no external fonts or external images (use system fonts).

Coder responsibilities
----------------------
- Create deterministic, minimal app that reads app/project-data.json and renders content in a readable, accessible way.
- Implement app/index.html (semantic), app/main.js (fetch + render + interactions), app/project-data.json (seed), .vscode/launch.json (workspace preview config).
- Implement graceful error handling when project-data.json is missing or malformed.
- Ensure no reliance on external CDNs or unpredictable resources.
- Keep implementation small and modular to be easy for learners to read.

Project-data.json  example schema and sample
---------------------------------------------
Schema (each item):
- id: string
- name: string
- owner: string
- status: "active" | "blocked" | "completed" | "on-hold"
- progress: number (0100)
- lastUpdated: ISO 8601 date string
- description: string (optional)
- tags: array of strings (optional)
- blockers: boolean (optional)
- nextStep: string (optional)

Small sample (seed, 6 items):
[
  {
    "id": "P-001",
    "name": "Onboarding redesign",
    "owner": "Alex J",
    "status": "active",
    "progress": 60,
    "lastUpdated": "2026-09-14T09:30:00Z",
    "description": "Redesign new user onboarding funnel to increase activation.",
    "tags": ["UX","frontend"],
    "blockers": false
  },
  {
    "id": "P-002",
    "name": "Billing API migration",
    "owner": "Priya S",
    "status": "blocked",
    "progress": 30,
    "lastUpdated": "2026-09-20T14:10:00Z",
    "description": "Move to external billing provider; waiting on API contract.",
    "tags": ["backend","api"],
    "blockers": true
  },
  {
    "id": "P-003",
    "name": "Docs hub",
    "owner": "Sam L",
    "status": "completed",
    "progress": 100,
    "lastUpdated": "2026-08-01T08:00:00Z",
    "description": "Public docs hub for integrations.",
    "tags": ["docs"],
    "blockers": false
  }
  // add 3 more varied examples
]

Dependencies between steps and files
------------------------------------
- Phase 1 (scaffolding) must complete before Phase 24 begin because files must exist in the app/ directory.
- Phase 2 (data) should be finished before Coder implements render logic in Phase 4  app/main.js expects predictable schema.
- Phase 3 (styles) and Phase 4 (index.html) can be worked on in parallel, provided the Designer agrees on class names and CSS hooks. To avoid conflicts: Designer must publish CSS hook list first.
- Phase 5 (interactions) depends on Phase 4 (markup + successful fetch/render baseline).
- .vscode/launch.json (Phase 6) can be added early by the Coder but must be validated after Phase 45 to ensure the preview opens correctly.
- docs/project-pulse-plan.md is independent and can be added anytime.

Work that can run in parallel
-----------------------------
- Designer: work on app/styles.css and docs/project-pulse-design.md.
- Coder: create app/index.html shell and initial app/project-data.json seed. These two must coordinate on the CSS hook names but otherwise parallel.
- Once stub markup exists, Designer can iterate on polishing styles while Coder implements main.js. The key coordination point is the agreed-upon class names.
- Creating .vscode/launch.json is independent and can occur in parallel with styling and coding.

Work that must be sequential
----------------------------
- Rendering logic (main.js) -> requires final project-data.json schema.
- Interaction wiring (filters/search/sort) -> requires stable markup in index.html.
- Final accessibility testing -> must happen after styling and interactions are complete.

Validation & tests (per phase)
------------------------------
Phase 1 validation
- Files exist: app/index.html, app/styles.css, app/project-data.json, .vscode/launch.json.
- Basic smoke test: open index.html in editor; it loads (no runtime errors in console caused by missing files).

Phase 2 validation
- JSON is valid (use JSON validator).
- Schema conformance: each entry has id, name, owner, status, progress, lastUpdated.
- Seed set contains at least one instance of each status and one with blockers=true.

Phase 3 validation
- CSS includes variables and class hooks named above.
- Use browser devtools to confirm classes exist and mobile breakpoints behave as described in design-spec.
- Contrast check: color contrast >= WCAG AA for body text vs background.

Phase 4 validation
- index.html renders without JS errors.
- main.js fetches app/project-data.json and creates DOM nodes corresponding to each project item.
- For N JSON items, there are N .project-card elements in DOM.

Phase 5 validation
- Search filters: typing a query filters results to matching name/owner/tags.
- Status filter: clicking status buttons shows only matching projects.
- Sort: sorting by progress or lastUpdated reorders displayed cards correctly.
- Keyboard accessibility: controls focusable with Tab; interactive items have visual focus indicators.
- Error behavior: temporarily rename project-data.json to trigger fetch error and confirm the friendly error banner appears.

Phase 6 validation
- VS Code: Opening the launch configuration should open index.html in a browser preview and the fetch should succeed.
- If using "Live Server" extension, index.html served as http://localhost:xxxx should fetch project-data.json without CORS or file-protocol problems.

Example .vscode/launch.json (suggested)
- The Coder should create this file (example):
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Open index.html (Edge)",
      "type": "pwa-msedge",
      "request": "launch",
      "file": "${workspaceFolder}/app/index.html",
      "cwd": "${workspaceFolder}/app"
    }
  ]
}
Notes:
- This opens index.html directly. If fetch fails because of file:// restrictions, the README must explain using Live Server or a tiny static server (e.g., npm's http-server) to serve files over http://localhost.

Edge cases & risks
------------------
- File protocol restrictions: fetching local JSON when index.html is opened via file:// may be blocked. Mitigation: recommend Live Server or provide alternative that embeds JSON as <script type="application/json" id="seed-data"> ... </script> as a fallback if running without a server. Primary implementation should use fetch and document the server requirement.
- Broken/invalid JSON: main.js must catch parse errors and show a clear UI error banner with guidance (e.g., "project-data.json is malformed").
- Missing fields: handle absent optional fields defensively. Required fields must be present; otherwise show placeholder values (e.g., owner: "Unassigned").
- Very large lists: ensure rendering is stable and that long descriptions don't break the layout (truncate with ellipsis and show full text on expand or tooltip).
- Long text / tags: design should allow wrapping or truncation; include max-height and read-more pattern for descriptions.
- Accessibility missteps: interactive elements must be keyboard-accessible and have ARIA labels. Test with screen reader and keyboard-only navigation.
- Timezones/date parsing: lastUpdated should be ISO; display in user's locale with a consistent format (e.g., YYYY-MM-DD or relative time like "3 days ago"). Document format choice.

Validation expectations (how to validate each phase)
----------------------------------------------------
- Use the browser console for JS errors.
- Use a JSON linter to validate project-data.json.
- Manual functional testing:
  - Open page in browser preview (via VS Code launch.json or Live Server)
  - Confirm card count equals JSON length
  - Test search, filter, and sort with expected results
  - Simulate an error (rename project-data.json) and confirm user-friendly error
- Accessibility checks:
  - Keyboard navigation: Tab through controls, activate with Enter/Space
  - Contrast: use color contrast check tools (browser extensions) to confirm AA
  - ARIA: check aria-live for update areas, role and aria-checked for toggle controls if used
- Responsive tests:
  - Inspect at 320px, 375px, 768px, 1024px and verify layout and readable text

Open questions (to be resolved before coding)
---------------------------------------------
1. Should app/main.js be a separate file or embedded in index.html? (Recommended: separate file to keep markup clean.)
2. Which statuses must be supported beyond the suggested list? ("active", "blocked", "completed", "on-hold")  confirm authoritative list.
3. Which filters are mandatory (search + status + sort) and which are optional (tags, owner filter)?
4. Preferred date display format: ISO date, human-readable, or relative time?
5. Should long descriptions be expandable (read-more) or truncated with a tooltip?
6. Are there any existing brand colors or typography constraints in the repository to match?

Final checklist (pre-merge / pre-demo)
--------------------------------------
- [ ] docs/project-pulse-plan.md saved in docs/ (this plan)
- [ ] app/ directory present with files: index.html, styles.css, project-data.json, main.js
- [ ] .vscode/launch.json present with cwd set to ${workspaceFolder}/app
- [ ] app/project-data.json validates against the defined schema and includes varied sample entries
- [ ] Designer has produced docs/project-pulse-design.md and app/styles.css with required hooks (.dashboard, .project-card)
- [ ] Index.html renders N project cards for N JSON items
- [ ] Search, filter, and sort behaviors work as specified
- [ ] Error and empty states tested and display user-friendly messages
- [ ] Accessibility checks completed (keyboard nav + focus + color contrast)
- [ ] README or docs contains instructions to preview using Live Server or launch.json
- [ ] All code and styles are deterministic (no external CDN, fonts, or network resources required for baseline view)

Notes to the Orchestrator (task splitting suggestions)
-----------------------------------------------------
- Assign Designer to: app/styles.css, docs/project-pulse-design.md (explicit CSS hook list and responsive spec).
- Assign Coder to: app/index.html, app/main.js, app/project-data.json, .vscode/launch.json, and README snippet.
- Keep file ownership strict (one owner per file) and require a short PR description linking to this project-pulse-plan.md.
- For parallel work: have Designer publish the hook list (class names) ASAP so Coder can start on markup and seed data concurrently.

If you want, I can now produce ready-to-drop starter file contents (index.html, main.js, styles.css skeleton, project-data.json seed, and suggested .vscode/launch.json) as separate text snippets for the Coder and Designer to use — but I will not modify the repository.