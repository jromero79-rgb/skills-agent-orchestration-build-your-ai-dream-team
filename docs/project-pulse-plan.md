# Project Pulse dashboard implementation plan

## Project Pulse goal

Build Mona's lightweight, static dashboard so contributors can quickly see active projects, owners, current status, recent activity, priority or risk, and a concise summary in a polished, readable layout. Follow the [project brief](../.github/project-pulse-brief.md). The repository does not yet contain an `app/` directory or a launch configuration; `.vscode/tasks.json` already exists and should remain unchanged. Keep the implementation dependency-free: load the top-level `projects` array from JSON in a small inline script in `app/index.html`, and serve the app over HTTP so that the browser can fetch that file.

## Responsibilities and file assignments

| Agent | Assigned files | Responsibility |
| --- | --- | --- |
| **Designer** | `app/styles.css` | Define the responsive visual system and information hierarchy. Style project cards, readable status and priority treatments, spacing, and accessible focus and contrast states. Agree with the Coder on the HTML class/element hooks before implementation; avoid editing HTML or data. |
| **Coder** | `app/index.html`, `app/project-data.json`, `.vscode/launch.json` | Create semantic dashboard markup and a small inline renderer that loads and displays project data; author representative data; and configure a deterministic VS Code launch option that serves from `app/` and opens `index.html`. Integrate against the Designer's agreed CSS hooks without editing `styles.css`. |
| **Orchestrator** | No implementation files | Coordinate the shared HTML/CSS contract, parallel work, integration, and validation; resolve any conflicts before work overlaps. |
| **Planner** | `docs/project-pulse-plan.md` | Define the phases, ownership, dependencies, and validation expectations in this plan. |

The project-data JSON should contain a top-level `projects` array. Each entry should include `name`, `owner`, `status`, `recentActivity`, `priority`, and a brief contributor-friendly `summary` to satisfy the brief. Agree on consistent status and priority values so cards and badges can be styled predictably.

## Implementation phases

1. **Agree on the interface contract — Orchestrator, then Designer and Coder.** Agree on the dashboard structure, semantic HTML, data fields, status/priority values, and CSS hooks (including `.dashboard` and `.project-card`). The Coder's inline renderer and the Designer's styles must use the same contract.
2. **Build independent assets in parallel.**
   - Designer creates `app/styles.css` using the agreed hooks and responsive/accessibility requirements.
   - Coder creates `app/project-data.json` with representative projects and the agreed schema, and `.vscode/launch.json` with a deterministic **Run Project Pulse Dashboard** configuration. The launch configuration must use `${workspaceFolder}/app` as its working directory, serve that directory over HTTP, and open `index.html` rather than a directory listing. Prefer the environment's built-in tooling; do not add a package dependency for a static server.
3. **Implement and integrate the page — Coder.** Create `app/index.html` using semantic, accessible structure and the agreed hooks. Add only the small inline JavaScript needed to fetch `project-data.json`, render project cards, and present a clear error state if loading fails or the data is invalid/empty. Connect the stylesheet and data file using paths that work when served from `app/`.
4. **Review and validate — Orchestrator with Designer and Coder.** Check the integrated UI, correct any contract mismatches within the assigned file scopes, and run the validations below.

## Dependencies and parallel work

- Step 1 is a prerequisite for implementation: shared class hooks and the data schema prevent mismatches across agent-owned files.
- After that contract is agreed, Designer's `styles.css` work can run in parallel with the Coder's JSON and launch configuration work because their file scopes are separate.
- The Coder's `index.html` integration depends on the agreed contract and the JSON schema. It can start before the parallel assets are complete only if the hooks and schema are fixed; final integration review waits for all assets.
- `app/index.html` links to `app/styles.css` and fetches `app/project-data.json`; therefore its class hooks must match the stylesheet and its renderer must match the JSON schema. `.vscode/launch.json` depends on the app entry point and must serve from `app/` so those relative paths and the JSON fetch work over HTTP.
- Final validation depends on all four assigned files being present and integrated. No agents should edit another agent's assigned file without an Orchestrator-coordinated handoff.

## Validation expectations

- Confirm all assigned files exist and `.vscode/tasks.json` remains unchanged.
- Parse `app/project-data.json` with a JSON parser; verify `projects` is an array and each entry has the required fields with usable values.
- Check that `index.html` references the stylesheet and JSON correctly, uses the agreed CSS hooks, and renders the project name, owner, status, recent activity, priority/risk, and summary.
- Review semantic structure, keyboard access, visible focus, contrast, responsive behavior, and error/empty-data feedback.
- Open **Run Project Pulse Dashboard** in the Codespace. Verify it serves the `app/` directory, opens `index.html` (not a directory listing), successfully loads the JSON, and displays the cards. Check browser/terminal output for loading errors.
- Since the repository has no existing app test suite or package manifest, use these focused checks and avoid introducing unrelated tooling or dependencies.

## Assumptions and open questions

- The brief does not prescribe project examples, status/priority vocabularies, or a visual theme. Use representative contributor-friendly data and a consistent set of values, and let the Designer choose a clear theme.
- The launch setup can use an HTTP server available in the Codespace without adding a dependency. If the expected server command or VS Code launch type is unavailable, the Coder should report the limitation and coordinate a compatible alternative with the Orchestrator.
- No blocking questions remain; the plan keeps the deliverable to the requested app files and launch configuration.
