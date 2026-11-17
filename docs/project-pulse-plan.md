# Project Pulse Dashboard Plan

## Goal

Build a lightweight static dashboard that helps Mona's contributors quickly see
which projects are active, who owns them, each project's status, recent
activity, priority or risk, and a short contributor-friendly summary. The
finished experience should be a polished, accessible, responsive set of project
cards—not a bare page or a server directory listing—and should be runnable from
VS Code's **Run Project Pulse Dashboard** configuration.

## Responsibilities

- **Orchestrator:** Coordinate the Designer and Coder, keep file ownership
  explicit, respect the dependencies below, integrate their work, and check
  the finished preview. Do not implement the dashboard or perform Git
  operations.
- **Designer:** Define the card layout, information hierarchy, readable
  spacing and typography, status and priority treatments, responsive behavior,
  and accessibility requirements. Give the Coder concrete recommendations;
  work within any file scope explicitly assigned. Do not stage, commit, or
  push.
- **Coder:** Implement the assigned static app and launch files, follow the
  agreed design and data contract, keep behavior deterministic, and report
  validation results and remaining risks. Do not stage, commit, or push.

## Phases and file assignments

### 1. Agree on the design and data contract

- **Designer:** Specify a clear dashboard heading, card content order,
  recognizable status badges and priority/risk treatment, readable contrast
  and spacing, semantic/accessibility requirements, and narrow-screen layout.
- **Coder:** Confirm the data contract before implementation: a top-level
  `projects` array whose project objects contain `name`, `owner`, `status`,
  `recentActivity`, and `priority`. Include a concise contributor-friendly
  summary for each project as an additional `summary` field.
- **Orchestrator:** Resolve any ambiguity in the recommendations and pass the
  same design and data contract to implementation.
- **Files:** No application files are assigned in this phase.

### 2. Create independent data and preview configuration

- **Coder — `app/project-data.json`:** Add realistic data for multiple
  projects using the agreed `projects` array and required fields. Include a
  concise `summary` as agreed above; keep values useful to contributors and
  consistent with the statuses and priorities shown by the UI.
- **Coder — `.vscode/launch.json`:** Add strict JSON (no comments) with a
  launch configuration named **Run Project Pulse Dashboard**. Run
  `python3 -m http.server 5500` with `cwd` set to
  `${workspaceFolder}/app`. Configure `serverReadyAction` to open
  `http://localhost:%s/index.html`, so the browser opens the app rather than a
  directory listing.
- **Files:** These two files have separate scopes and can be created in
  parallel after the data contract is agreed. Neither depends on the other.

### 3. Implement and integrate the dashboard frontend

- **Coder — `app/index.html`:** Create the accessible document with the exact
  page title **Project Pulse**. Reference `styles.css` and load
  `project-data.json`; render visible project cards from the `projects` data,
  using the class `project-card` for each card. Show each project's name,
  owner, status, recent activity, priority, and summary.
- **Coder — `app/styles.css`:** Implement the Designer's polished card-based
  direction, including `.dashboard` and `.project-card` selectors,
  status/priority treatments, readable spacing, responsive layout,
  `border-radius`, and `box-shadow`. Ensure focus and text contrast remain
  usable.
- **Dependencies:** Start after the design recommendations and data contract
  are agreed and the JSON schema is fixed. Keep HTML and CSS implementation
  coordinated so class hooks, semantic markup, and responsive states match.
  The HTML/CSS pair can be developed concurrently only if the shared selectors
  and structure are agreed first; otherwise implement them together to avoid
  interface mismatches.

### 4. Validate the integrated result

- **Orchestrator and Coder:** Check the files together, run the preview, and
  resolve any integration issues before reporting completion.
- **Files:** Validation is limited to the four assigned deliverables; do not
  broaden scope or change unrelated worktree files.

## Dependencies and parallel/sequential decisions

1. The design and data contract must precede implementation of the UI.
2. In Phase 2, the JSON data and VS Code launch configuration are independent
   and may be produced in parallel.
3. Frontend work follows the data contract and Designer's recommendations.
   HTML and CSS may proceed in parallel only against an agreed markup/style
   interface; otherwise keep them coordinated or sequential.
4. Final browser validation depends on all four deliverables being present.
   Do not treat the launch configuration as validated until it serves from
   `app/` and opens `index.html`.

## Validation expectations

- **Data:** Parse `app/project-data.json` as JSON. Confirm it has a top-level
  `projects` array, contains multiple project objects, and every object has
  `name`, `owner`, `status`, `recentActivity`, and `priority`; confirm each
  also has a contributor-friendly `summary`.
- **Markup and rendering:** Confirm `app/index.html` has the exact title
  `Project Pulse`, references `styles.css` and `project-data.json`, and
  renders visible `.project-card` elements from the data. Verify cards show
  name, owner, status, recent activity, priority, and summary rather than
  relying on hard-coded duplicate content.
- **Accessibility and design:** Review semantic structure, meaningful
  headings, readable contrast and spacing, accessible status/priority
  information that is not conveyed by color alone, visible keyboard focus,
  and usable layout at narrow and wide viewport sizes. Confirm the result is
  a polished card-based dashboard, with `.dashboard` and `.project-card`
  styles, rounded corners, and shadows.
- **Launch configuration:** Parse `.vscode/launch.json` with a strict JSON
  parser (no comments). Confirm it contains **Run Project Pulse Dashboard**,
  uses `python3 -m http.server 5500`, sets `cwd` to
  `${workspaceFolder}/app`, and opens
  `http://localhost:%s/index.html` through `serverReadyAction`.
- **End-to-end preview:** Start the named VS Code launch configuration and
  confirm the browser opens the Project Pulse UI at `index.html`, displays
  project cards, and does not show a directory listing. Stop the preview
  server after checking it.
