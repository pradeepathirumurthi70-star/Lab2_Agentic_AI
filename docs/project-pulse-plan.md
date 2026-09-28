# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona's Project Pulse as a small static dashboard for contributors. It should show multiple projects, their owners, status, recent activity, priority or risk, and a short contributor-friendly summary. Use the existing brief's required data fields and the exercise's exact selectors, launch name, and preview behavior. Keep the app dependency-free; no package manifest, framework, or automated frontend test suite currently exists.

## Responsibilities

- **Planner:** This plan identifies file ownership, dependencies, parallel work, edge cases, and validation.
- **Designer:** Define information hierarchy, card and badge treatment, accessible color and typography, responsive behavior, and loading, empty, and error-state guidance. Deliver design decisions to the Orchestrator; do not edit implementation files unless separately assigned.
- **Coder:** Own implementation of all four app and launch files, integrate the Designer's guidance, keep the required data contract, and report validation results.
- **Orchestrator:** Coordinate the handoffs, enforce file ownership and ordering, review integration, and surface blockers.

## Ordered implementation steps and file assignments

1. **Agree on the design and data contract.** Designer defines the dashboard's visual hierarchy and responsive/accessibility guidance. Orchestrator confirms the data shape with Coder: a top-level `projects` array; every project has `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a concise `summary` field as well to meet the brief's contributor-summary requirement without changing the required fields.
2. **Prepare independent inputs.** Coder creates representative sample records in `app/project-data.json` and the preview configuration in `.vscode/launch.json`. Designer can work on the design guidance at the same time because these files do not overlap.
3. **Implement and integrate the dashboard.** After receiving the design guidance and agreed data contract, Coder creates:
   - `app/index.html`: accessible page titled exactly "Project Pulse"; references `styles.css` and loads `project-data.json`; renders one `.project-card` per record with the project's name, owner, status, recent activity, priority, and summary.
   - `app/styles.css`: responsive styling with `.dashboard` and `.project-card` selectors, readable spacing, visible status/priority treatments, rounded cards (`border-radius`), and subtle depth (`box-shadow`).
   - `app/project-data.json`: valid JSON with multiple sample projects and all required fields, plus `summary`.
   - `.vscode/launch.json`: strict JSON with a **Run Project Pulse Dashboard** configuration that serves from `${workspaceFolder}/app` using `python3 -m http.server 5500` and opens `http://localhost:%s/index.html` through `serverReadyAction`. Use a launch type that supports running a terminal command and opening a browser; match the server-ready pattern to Python's actual startup output.
4. **Review the integrated result.** Orchestrator checks the files against the brief, workflow checks, and validation expectations below before handing off.

## Dependencies and work ordering

- The data contract and Designer guidance must be agreed before Coder integrates page markup and styling.
- Designer's guidance and Coder's initial `project-data.json` and `launch.json` work can run **in parallel** once the shared data contract and launch requirements are clear; their file scopes do not overlap.
- `index.html` and final CSS integration should follow the design handoff and data contract. Keep Coder as the sole editor of implementation files to avoid conflicts.
- Integrated validation and browser preview are **sequential** and happen after all four files exist.

## Edge cases and implementation considerations

- A browser opened with `file://` may block JSON `fetch`; preview must use the configured HTTP server.
- Handle failed or invalid JSON loading visibly rather than leaving an unexplained blank dashboard. Define an empty-project state as well.
- Render data as text, not interpolated HTML, so project values cannot become markup.
- Status and priority must remain understandable through text and not color alone; preserve contrast and visible keyboard focus.
- Use a ready pattern that matches `python3 -m http.server` output. If port 5500 is already occupied or Python is unavailable, the server may fail and the browser may not open; report that clearly rather than treating it as a successful launch.
- Avoid dependencies or build steps: the repository currently provides no app framework, package manifest, or frontend test runner.

## Validation expectations

- Confirm all four assigned files exist.
- Parse `app/project-data.json` and `.vscode/launch.json` with `python3 -m json.tool`.
- Check `index.html` references both assets, contains the exact title, and provides project-card markup showing the required fields.
- Check CSS includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, and inspect the narrow-screen layout and focus/contrast choices.
- Confirm every project record has `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`; check rendering for populated and empty/error data states.
- Run **Run Project Pulse Dashboard** in VS Code. Confirm the browser opens `index.html`, the page is not a directory listing, project data loads, and cards display. Stop the preview server after the smoke test.
- The repository's Step 3 workflow checks file presence, required keyphrases, and JSON syntax. These checks do not prove that the preview runs or that the UI is accessible, so retain the manual smoke test.

## Open questions

- The brief does not provide authoritative project records. Use clearly illustrative sample data unless Mona supplies real project details.
- The brief asks for a short summary but does not list a `summary` key among the required fields. This plan adds it as an additional field while preserving all five required fields.
