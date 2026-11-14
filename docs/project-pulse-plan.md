# Project Pulse dashboard implementation plan

## Goal

Create a small, static dashboard that helps contributors scan active projects, owners, status, recent activity, and priority. The result should be a polished, accessible, responsive frontend and a VS Code launch configuration that serves the `app/` directory and opens `index.html`, rather than displaying a directory listing.

## Repository context and dependencies

- The project brief is `.github/project-pulse-brief.md`; the team roles are described in `docs/agent-team.md`.
- The `app/` directory exists but has no implementation files yet. There is no existing frontend framework, package manifest, or build/test setup to extend.
- Keep this a static app: use HTML, CSS, JSON, and browser-native JavaScript as needed. No third-party runtime dependencies or package installation are required.
- A local HTTP server is needed to load JSON in the browser. The launch configuration should use Python 3's standard library server on port `5500` with its working directory set to `${workspaceFolder}/app`.

## Roles

- **Planner:** Establish this file contract, task order, dependencies, parallel work boundaries, and completion checks.
- **Designer:** Define the visual hierarchy, responsive behavior, color/contrast, readable spacing, project-card and status/priority treatments, and accessible interaction/presentation requirements. Implement only the assigned stylesheet and report the class hooks expected from the HTML.
- **Coder:** Implement the data-to-UI connection and runnable support files within the assigned scope. Keep the data schema and HTML/CSS hooks consistent with the brief and Designer's contract; validate the JSON and launch configuration.
- **Orchestrator:** Assign the file scopes below, coordinate the shared contract before parallel work, integrate the results, and confirm the preview and checks pass.

## Phases and file assignments

### Phase 1 — Agree on the interface contract

Before parallel implementation, agree on the card fields and selectors: each card presents `name`, `owner`, `status`, `recentActivity`, and `priority`; the page provides a `.dashboard` container and each card uses `.project-card`. Define clear text labels and status/priority treatments so styling and rendering align.

### Phase 2 — Implement in parallel

Once the contract is agreed, the following non-overlapping assignments can proceed in parallel:

| File | Owner | Assignment |
| --- | --- | --- |
| `app/styles.css` | Designer | Style the dashboard and project cards; include `.dashboard` and `.project-card` hooks, clear status and priority distinctions, readable typography and spacing, rounded card treatment and subtle shadows, visible focus where interactive elements exist, and responsive layouts. Preserve legible contrast and avoid relying on color alone to convey status. |
| `app/index.html` | Coder | Create the semantic page with the exact title “Project Pulse”, link `styles.css`, and load `project-data.json` to render visible project cards from the top-level `projects` array. Use the agreed CSS hooks and display every required field. Provide a clear loading and recoverable error state if data cannot be loaded. |
| `app/project-data.json` | Coder | Supply representative contributor-friendly project records in a top-level `projects` array. Each object must include `name`, `owner`, `status`, `recentActivity`, and `priority`; use meaningful, concise values suitable for rendering. |
| `.vscode/launch.json` | Coder | Add strict JSON (no comments) with a configuration named `Run Project Pulse Dashboard`. Run `python3 -m http.server 5500` from `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. This ensures the browser opens the dashboard page, not a directory listing. |

### Phase 3 — Integrate and verify

Coder and Designer resolve any mismatches in selectors or content presentation within their assigned files. The Orchestrator confirms that the page loads the supplied data and the launch configuration matches the expected server behavior. Keep any necessary changes within the four assigned implementation files.

## Dependencies and parallel work decisions

- Phase 1 precedes implementation: HTML field names and class hooks must be agreed before Designer and Coder work independently.
- `app/index.html` depends on the JSON schema and CSS hooks, but can be implemented in parallel with the other files once those contracts are fixed.
- `app/project-data.json` and `app/styles.css` are independent of each other and can be created in parallel.
- `.vscode/launch.json` is independent of page styling and data content; it can be created in parallel, provided it serves from `app/` and opens `index.html`.
- Phase 3 is sequential after all four files exist because integration and preview validation require the complete app.

## Edge cases and implementation notes

- Browser `fetch` of JSON should be tested over HTTP, not by opening `index.html` directly as a `file://` URL.
- Handle an empty `projects` array gracefully and provide a useful message if the JSON request fails.
- Render project-provided text as text, not injected HTML.
- Keep status and priority understandable through text labels as well as color, and ensure the card layout remains readable on narrow screens.
- Keep launch JSON valid and deterministic; use the configured app working directory so the server root is not the repository root.

## Validation expectations

1. Parse `app/project-data.json` with `python3 -m json.tool`; confirm the top-level `projects` key and all five required fields on each record.
2. Parse `.vscode/launch.json` with `python3 -m json.tool`; confirm the named configuration, Python server command, app working directory, and `index.html` browser URL.
3. Inspect `app/index.html` for the exact page title, stylesheet and JSON references, semantic content, agreed selectors, and visible rendering of all five fields. Confirm it includes understandable loading, empty, and request-error states.
4. Inspect `app/styles.css` for the required selectors, responsive layout, readable contrast and spacing, and card/status/priority styling.
5. Run **Run Project Pulse Dashboard** in VS Code. Confirm the browser opens the Project Pulse UI at `/index.html`, project cards render from JSON, and no directory listing appears. Check a narrow viewport and keyboard focus/readability where applicable.
6. No build or automated test command currently exists for the app. Do not add a framework or dependencies solely for validation; use the repository's existing exercise checks as appropriate after implementation.

## Open questions

- The brief does not prescribe a project count, project names, or a specific visual theme; use a small representative dataset and a coherent, contributor-friendly visual direction.
- The brief does not specify a browser target. Prefer broadly supported HTML, CSS, and browser-native JavaScript features.
