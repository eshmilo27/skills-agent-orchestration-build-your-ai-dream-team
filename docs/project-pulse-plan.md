# Project Pulse implementation plan

## Summary

Build a lightweight static Project Pulse dashboard for contributors. The UI
will show multiple project cards with each project's name, owner, status, recent
activity, priority, and contributor-friendly summary. The app will load data
from JSON and run through a VS Code launch profile that serves `app/` and opens
`index.html`.

## Ordered steps

1. **Confirm requirements and contracts**
   - Planner reviews `.github/project-pulse-brief.md`, agent responsibilities,
     and repository validation workflows.
   - Define the JSON schema, dashboard markup hooks, responsive behavior,
     accessibility expectations, and launch configuration requirements.

2. **Create the project data**
   - **Coder assignment:** `app/project-data.json`
   - Add a valid top-level `projects` array.
   - Include multiple realistic projects with `name`, `owner`, `status`,
     `recentActivity`, and `priority`.
   - Keep values deterministic and suitable for visible card rendering.

3. **Define the visual system**
   - **Designer assignment:** `app/styles.css`
   - Create polished responsive dashboard styling with `.dashboard` and
     `.project-card` selectors.
   - Include readable typography, spacing, status and priority treatment,
     `border-radius`, `box-shadow`, strong contrast, keyboard-focus states, and
     mobile layout behavior.
   - Ensure the design clearly communicates Project Pulse on first load.

4. **Implement the dashboard page**
   - **Coder assignment:** `app/index.html`
   - Add the exact page title and visible Project Pulse heading.
   - Reference `styles.css` and `project-data.json`.
   - Load the JSON data and render one `.project-card` per project.
   - Render each project's name, owner, status, `recentActivity`, priority, and
     summary with semantic, accessible markup.
   - Handle loading and data-load errors with readable UI feedback.

5. **Add the runnable preview configuration**
   - **Coder assignment:** `.vscode/launch.json`
   - Create strict JSON with no comments.
   - Add the exact configuration name `Run Project Pulse Dashboard`.
   - Use `python3 -m http.server 5500`.
   - Set `cwd` to `${workspaceFolder}/app`.
   - Configure `serverReadyAction` to open
     `http://localhost:%s/index.html`, ensuring the dashboard opens instead of
     a directory listing.

6. **Integrate and review**
   - **Orchestrator responsibility:** review Designer and Coder outputs
     together.
   - Check that HTML class names and data fields match the CSS and JSON
     contracts.
   - Resolve visual, accessibility, or launch-configuration inconsistencies.

## File assignments

| File | Owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder, informed by Designer | Accessible page structure, data loading, card rendering, and status/activity/priority display |
| `app/styles.css` | Designer | Visual hierarchy, responsive layout, card styling, badges, focus states, `.dashboard`, and `.project-card` |
| `app/project-data.json` | Coder | Valid deterministic project dataset with the required schema |
| `.vscode/launch.json` | Coder | Strict JSON launch profile serving `app/` and opening `index.html` |
| `docs/project-pulse-plan.md` | Planner | This implementation plan |

## Dependencies

- `app/project-data.json` defines the fields consumed by `app/index.html`.
- Designer's CSS hooks and markup recommendations must be agreed before Coder
  finalizes the HTML structure.
- `app/index.html` depends on both `app/styles.css` and
  `app/project-data.json`.
- `.vscode/launch.json` depends on the final app location and entry point, but
  does not require the visual implementation to be complete.
- Integration validation depends on all four implementation files existing.

## Parallel work decisions

- **Can run in parallel:** Designer can define `app/styles.css` while Coder
  creates `app/project-data.json`; these files have no direct write overlap.
- **Can run in parallel:** Coder can draft `.vscode/launch.json` while Designer
  works on CSS.
- **Must be sequential:** Coder finalizes `app/index.html` after the data
  schema and CSS hooks are available.
- **Must be sequential:** Orchestrator integration review and browser
  validation occur only after all four implementation files are complete.
- **Must be sequential:** Any launch or markup corrections are made before
  final validation.

## Edge cases

- Empty or malformed `projects` data should show a clear empty/error state
  rather than a blank page.
- Missing optional summary content must not break card layout.
- Long project names, owner names, and activity text must wrap without
  overflow.
- Status and priority colors must not be the only way information is conveyed.
- The layout must remain usable on narrow screens and with keyboard navigation.
- The launch configuration must open `index.html`, not the server directory
  root.
- JSON files must contain no comments or trailing commas.

## Validation expectations

- Run `python3 -m json.tool app/project-data.json`.
- Run `python3 -m json.tool .vscode/launch.json`.
- Confirm `app/index.html` references `styles.css` and `project-data.json`,
  contains `Project Pulse`, uses `.project-card`, and renders status,
  `recentActivity`, and priority.
- Confirm `app/styles.css` contains `.dashboard`, `.project-card`,
  `border-radius`, and `box-shadow`.
- Confirm the data file has the required top-level `projects` array and fields.
- Start **Run Project Pulse Dashboard** and verify that
  `http://localhost:5500/index.html` displays the dashboard.
- Review responsive layout, keyboard focus, contrast, loading behavior, and
  error behavior in the browser.
- Run `bash scripts/validate-exercise.sh` after the implementation is present.

## Open questions

None blocking. This plan assumes Python 3 and standard VS Code browser/debug
support are available in the Codespace.