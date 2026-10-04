# Mona's Project Pulse Dashboard Implementation Plan

## Summary

Build a polished, static **Project Pulse** dashboard in the currently minimal repository scaffold. The repository contains agent definitions, exercise documentation, GitHub Actions checks, and a Codespace startup script, but it does not currently contain an `app/` implementation or `.vscode/launch.json`.

The implementation should use a dependency-free frontend composed of:

- `app/index.html` for dashboard structure and rendering behavior
- `app/styles.css` for responsive visual design
- `app/project-data.json` for deterministic project data
- `.vscode/launch.json` for a repeatable local preview configuration

The dashboard must clearly display project cards with project name, owner, status, recent activity, and priority. It should look like a finished dashboard rather than a bare HTML page and should be accessible, responsive, and easy to launch from the Codespace.

## Repository Evidence and Constraints

- `README.md` identifies the repository as the “Agent Orchestration Build Your AI Dream Team” exercise and links to the exercise issue.
- `docs/agent-team.md` defines the four-agent team:
  - Planner: repository research and planning
  - Orchestrator: phase breakdown and coordination
  - Designer: UI/UX, accessibility, hierarchy, interaction flow, and visual styling
  - Coder: implementation, support files, and validation
- `.github/agents/designer.agent.md` explicitly requires:
  - A polished dashboard
  - Visible project cards
  - Status badges
  - Clear priority treatment
  - Readable spacing and responsive behavior
  - `.dashboard` and `.project-card` CSS hooks
  - Rounded corners, shadows, contrast, and clear typography
- `.github/agents/coder.agent.md` requires:
  - Clear, deterministic, testable implementation
  - Explicit errors
  - Strict JSON for `.vscode/launch.json`
  - `cwd` set to `${workspaceFolder}/app`
  - A launch configuration that opens `index.html`
- `.github/workflows/3-step.yml` is the implementation gate. It checks for:
  - `app/index.html`
  - `app/styles.css`
  - `app/project-data.json`
  - `.vscode/launch.json`
  - Project Pulse content
  - References to `styles.css` and `project-data.json`
  - `project-card`, `status`, `recentActivity`, and `priority` markup
  - `.dashboard`, `.project-card`, `border-radius`, and `box-shadow` styling
  - A JSON data object containing `projects`, `name`, `owner`, `status`, `recentActivity`, and `priority`
  - Valid JSON in both JSON files
  - A launch configuration named `Run Project Pulse Dashboard` containing `index.html`
- `.vscode/tasks.json` only opens `.devcontainer/postStart.sh` when the folder opens; it is not an application launch configuration.
- `.devcontainer/postStart.sh` starts GitHub Copilot CLI with broad exercise permissions. It does not provide a web server or frontend runtime.
- No existing package manifest, framework, build system, JavaScript entry file, or application test suite is present in the inspected repository.
- The requested application files and `.vscode/launch.json` do not yet exist.

## Ordered Implementation Steps

### 1. Confirm the implementation contract

**Owner:** Orchestrator, with Planner input  
**Files:** No code changes; use this plan as the contract.

Confirm that the implementation remains a static, dependency-free dashboard unless new repository evidence or user requirements introduce a framework or backend. Establish the shared data and markup contract before overlapping implementation work begins:

- Data root: `{ "projects": [...] }`
- Each project includes:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- HTML must expose semantic project-card markup and use the required field names.
- CSS must expose `.dashboard` and `.project-card`.
- The preview must run from the `app` working directory and open `index.html`.

### 2. Produce the visual and accessibility direction

**Owner:** Designer  
**Assigned file:** `app/styles.css`  
**Supporting responsibility:** Provide the Coder with the expected HTML class and data hooks before implementation begins.

Define and implement the visual system in `app/styles.css`:

- Dashboard-level layout and responsive grid behavior
- Clear page title and supporting context
- Project-card hierarchy
- Status badges with meaningful color and text contrast
- Priority treatment that remains understandable without color alone
- Owner and recent-activity presentation
- Rounded corners and restrained shadows
- Consistent spacing, typography, and focus states
- Mobile behavior for narrow viewports
- Reduced-motion-safe interactions if transitions or hover effects are used
- Empty-state and loading-state styling if the Coder includes those states in the HTML

The Designer should avoid modifying `app/index.html`, `app/project-data.json`, or `.vscode/launch.json`. The stylesheet should be usable by the Coder’s agreed markup contract.

### 3. Define deterministic Project Pulse data

**Owner:** Coder  
**Assigned file:** `app/project-data.json`

Create valid, human-readable JSON containing a non-empty `projects` array. Each project should have representative values for:

- `name`
- `owner`
- `status`
- `recentActivity`
- `priority`

Use a small, varied dataset that demonstrates multiple statuses and priority levels without introducing external services or unstable timestamps. Keep the values suitable for a dashboard demonstration and consistent with the labels rendered by the HTML.

The data file must remain strict JSON: no comments, trailing commas, or JavaScript expressions.

### 4. Implement the dashboard document and rendering behavior

**Owner:** Coder  
**Assigned file:** `app/index.html`

Create a self-contained static page that:

- Includes a meaningful document title and visible “Project Pulse” dashboard heading
- References `styles.css`
- References `project-data.json`
- Uses semantic HTML landmarks and headings
- Provides a dashboard container with the `.dashboard` hook
- Renders project cards using the `.project-card` hook
- Displays each project’s `name`, `owner`, `status`, `recentActivity`, and `priority`
- Provides accessible labels or text for status and priority
- Handles data loading deterministically
- Reports a useful visible error if the JSON cannot be loaded or parsed instead of silently showing an empty successful state
- Provides an accessible loading or empty state where appropriate

Because no JavaScript entry file or package manifest exists, the initial plan assumes the rendering logic will be placed in a small inline script in `app/index.html`. This keeps the requested file scope dependency-free. If the Orchestrator later authorizes a separate JavaScript file, that file must be added to the explicit assignment and workflow scope.

The Coder should preserve the Designer’s class and data-hook contract rather than duplicating or renaming it.

### 5. Add the local launch configuration

**Owner:** Coder  
**Assigned file:** `.vscode/launch.json`

Create strict JSON with a deterministic configuration named exactly:

```text
Run Project Pulse Dashboard
```

The configuration must:

- Use a suitable browser-preview mechanism available in the Codespace/editor environment
- Set `cwd` to `${workspaceFolder}/app`
- Open `index.html`
- Avoid opening a directory listing
- Use a stable local port or preview command if a web server is required for JSON loading

A static browser may block `fetch("project-data.json")` when opening `index.html` directly from `file://`. Therefore, the preferred launch configuration should serve the `app` directory over HTTP before opening `index.html`, provided the required preview mechanism is available in the environment. The exact debugger type or server command is uncertain because the repository does not include a frontend toolchain or launch precedent.

### 6. Integrate and review the four assigned files

**Owner:** Orchestrator  
**Files reviewed:** `app/index.html`, `app/styles.css`, `app/project-data.json`, `.vscode/launch.json`

Check that:

- HTML class names match CSS selectors.
- HTML field names match the JSON schema.
- All required data fields are rendered.
- The data-loading path works with the configured `cwd`.
- The launch configuration opens the dashboard rather than a directory.
- The Designer’s responsive and accessibility requirements remain intact.
- No unassigned files or dependencies were introduced without agreement.

### 7. Validate the implementation

**Owner:** Coder, verified by Orchestrator  
**Files:** All four implementation files

Run the repository’s targeted checks and a manual browser preview. Resolve any mismatch before handoff.

## File Assignments

| File | Primary owner | Responsibilities |
|---|---|---|
| `app/index.html` | Coder | Semantic dashboard document, Project Pulse heading, data loading/rendering, project-card markup, visible error/loading/empty states |
| `app/styles.css` | Designer | Complete visual system, responsive layout, accessibility states, `.dashboard`, `.project-card`, badges, priority styling, rounded cards, shadows |
| `app/project-data.json` | Coder | Valid deterministic `projects` dataset with `name`, `owner`, `status`, `recentActivity`, and `priority` |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named `Run Project Pulse Dashboard`, `cwd` set to `${workspaceFolder}/app`, opens `index.html` |
| `docs/project-pulse-plan.md` | Planner | This implementation plan only; no application implementation |

## Designer Responsibilities

The Designer owns the visual and interaction-quality direction within `app/styles.css` and should:

1. Establish a clear information hierarchy for the dashboard title, project list, status, priority, owner, and recent activity.
2. Ensure cards are visually distinct and scannable.
3. Use status and priority styling that remains understandable through text, labels, icons, or other non-color cues.
4. Define responsive behavior for desktop, tablet, and narrow mobile widths.
5. Include keyboard-visible focus states and sufficient contrast.
6. Provide CSS hooks and markup expectations to the Coder without modifying the Coder-owned files.
7. Review the integrated HTML for visual consistency without taking ownership of the HTML file.

## Coder Responsibilities

The Coder owns the functional and support files and should:

1. Create `app/index.html` and render data from `app/project-data.json`.
2. Create valid deterministic project data in `app/project-data.json`.
3. Preserve the agreed Designer class names, especially `.dashboard` and `.project-card`.
4. Handle loading, malformed data, empty data, and fetch failures explicitly.
5. Create `.vscode/launch.json` with strict JSON, the required name, the required working directory, and an `index.html` target.
6. Keep the implementation dependency-free unless a dependency is demonstrably required and approved.
7. Validate syntax, data loading, rendered fields, and launch behavior.
8. Avoid modifying `app/styles.css` unless the Orchestrator explicitly assigns a narrowly scoped integration fix.

## Dependencies

### Required dependencies

- A browser capable of displaying the dashboard.
- A local HTTP-serving or preview mechanism if the page loads JSON with `fetch`.
- Python 3 may be available for ad hoc validation and a simple local server, but it is not declared as a repository dependency.
- VS Code or compatible Codespace launch support for `.vscode/launch.json`.

### No currently declared application dependencies

The repository contains no inspected package manifest or frontend framework configuration. The initial implementation should therefore avoid npm packages, build steps, bundlers, and external APIs.

### Data and rendering dependency

`app/index.html` depends on the schema and file path in `app/project-data.json`. The Coder must finalize the data shape before completing rendering behavior.

### Design and markup dependency

The Coder’s HTML structure must consume the Designer’s class and hook contract. The Designer should establish that contract before the Coder finalizes the HTML.

## Parallel Work Decisions

### Work that can run in parallel

After the shared contract is agreed:

- Designer can implement `app/styles.css`.
- Coder can create `app/project-data.json`.
- Coder can draft `.vscode/launch.json` based on the known required launch behavior.
- Planner or Orchestrator can prepare validation criteria from `.github/workflows/3-step.yml`.

These tasks have separate primary files and do not require each other’s implementation details.

### Work that must run sequentially

1. Establish the data schema and CSS/HTML hook contract before final integration.
2. Complete the Designer’s markup expectations before the Coder finalizes `app/index.html`.
3. Complete `app/project-data.json` before validating the HTML’s data rendering.
4. Complete `app/index.html` before finalizing or validating the launch target.
5. Run integration validation after all four files exist.
6. Resolve any browser or launch issues before declaring the dashboard complete.

The Designer and Coder should not edit the same file concurrently. In particular, CSS integration fixes should be coordinated through the Orchestrator rather than silently changing ownership.

## Edge Cases and Risks

- **Direct file loading:** Opening `index.html` with `file://` may prevent JSON loading. Prefer an HTTP preview configuration.
- **Malformed JSON:** The page must show an explicit, user-readable error rather than silently rendering no cards.
- **Missing fields:** Rendering should use clear fallback text or report invalid project records; it must not produce broken labels or undefined text.
- **Empty project list:** Show an intentional empty state rather than an empty dashboard with no explanation.
- **Unknown status or priority:** Preserve the value as readable text and apply a safe fallback style.
- **Accessibility:** Do not communicate status or priority by color alone. Ensure heading structure, keyboard focus, contrast, and meaningful labels.
- **Responsive overflow:** Long project names, owner names, and activity text must wrap or truncate safely without horizontal scrolling.
- **Reduced motion:** Avoid motion-dependent feedback and respect `prefers-reduced-motion` if transitions are added.
- **Launch compatibility:** The repository does not reveal which VS Code browser debugger or server extension is installed. The chosen configuration must be tested in the actual Codespace.
- **No build tooling:** Do not assume npm scripts, a framework, or a test runner that is not present.
- **Stale data:** Use deterministic sample content; avoid depending on live GitHub APIs or current time.
- **Scope drift:** Do not add unrelated app files, dependencies, or configuration without updating the plan and assignments.

## Validation Expectations

### Automated repository checks

Run the checks represented in `.github/workflows/3-step.yml`:

- Confirm all four required files exist.
- Parse `app/project-data.json` as valid JSON.
- Parse `.vscode/launch.json` as valid JSON.
- Confirm `app/index.html` contains:
  - `Project Pulse`
  - `styles.css`
  - `project-data.json`
  - `project-card`
  - `status`
  - `recentActivity`
  - `priority`
- Confirm `app/styles.css` contains:
  - `.dashboard`
  - `.project-card`
  - `border-radius`
  - `box-shadow`
- Confirm `app/project-data.json` contains:
  - `projects`
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- Confirm `.vscode/launch.json` contains:
  - `Run Project Pulse Dashboard`
  - `index.html`

### Manual browser validation

Using the configured launch action:

1. Open the dashboard from the repository workspace.
2. Confirm the launch opens `index.html`, not an app directory listing.
3. Confirm project cards render from `project-data.json`.
4. Confirm every card displays name, owner, status, recent activity, and priority.
5. Confirm loading, empty, and failure behavior is understandable if those conditions are simulated.
6. Resize the viewport to verify responsive layout and text wrapping.
7. Navigate with the keyboard to verify focus visibility and usable interaction order.
8. Inspect contrast and verify status/priority remain understandable without color perception.
9. Check the browser console for fetch, parsing, or runtime errors.

### Documentation validation

Ensure the final implementation handoff references:

- `app/index.html`
- `app/styles.css`
- `app/project-data.json`
- `.vscode/launch.json`
- The launch configuration name
- Validation performed and any remaining environment-specific uncertainty

## Open Questions and Uncertainties

1. **Launch mechanism:** The repository does not identify whether a browser debugger extension or a preferred local server command is installed. The Orchestrator should select and test the most compatible configuration in the Codespace.
2. **JavaScript file scope:** No separate script file is requested or currently present. This plan assumes inline rendering logic in `app/index.html`; adding `app/app.js` or another support file would require an explicit scope decision.
3. **Visual content requirements:** No separate product brief or brand specification was available in the inspected repository. The Designer should use a polished, accessible dashboard treatment while avoiding unsupported brand assumptions.
4. **Interaction requirements:** No filtering, sorting, editing, or navigation behavior is specified. The initial implementation should focus on reliable display and clear states rather than inventing complex interactions.
5. **Browser support target:** No browser matrix is documented. Prefer broadly supported HTML, CSS, and JavaScript without framework-specific or experimental features.
