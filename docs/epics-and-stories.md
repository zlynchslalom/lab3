# Epics, Stories, Acceptance Criteria, and Technical Requirements

This document maps Epics → Stories → Acceptance Criteria and lists Technical Requirements necessary to implement the changes described in `docs/prd-todo.md`. Content is scoped for MVP unless marked Post‑MVP.

## Epic: Task Data Model

- Story: Add `dueDate` field to task model
  - Acceptance Criteria:
    - `dueDate` accepts an ISO `YYYY-MM-DD` string or is absent
    - Invalid date values are ignored and treated as absent

- Story: Add `priority` enum to task model
  - Acceptance Criteria:
    - `priority` must be one of `P1`, `P2`, or `P3`
    - If missing or invalid, `priority` defaults to `P3`

- Story: Enforce required `title` field
  - Acceptance Criteria:
    - Creating or saving a task without `title` is prevented by UI validation

### Technical Requirements (Data Model)

- Data shape (front-end representation):
  - `id` (string|number), `title` (string, required), `description` (string, optional), `due_date` or `dueDate` (ISO `YYYY-MM-DD`, optional), `priority` (`P1|P2|P3`, default `P3`), `completed` (boolean)
- Normalize field names: follow existing code which uses `due_date` (snake_case) in `TaskForm` and `TaskList`; keep a normalization layer if converting to/from `dueDate`.
- Validation rules implemented client-side: title required, priority coerced to `P3` when invalid, due date validated as ISO `YYYY-MM-DD` and ignored if invalid.

## Epic: Task Creation and Editing

- Story: Add due date input to `TaskForm`
  - Acceptance Criteria:
    - `TaskForm` exposes a date input; saving a valid ISO date stores it on the task

- Story: Add priority selector to `TaskForm`
  - Acceptance Criteria:
    - `TaskForm` exposes a selector for `P1`, `P2`, `P3`; missing selection defaults to `P3`

- Story: Allow editing `dueDate` and `priority` on existing tasks
  - Acceptance Criteria:
    - Editing and saving updates `due_date` / `priority`; changes persist after reload

### Technical Requirements (Creation & Editing)

- Update `packages/frontend/src/TaskForm.js` to include a `priority` selector (MUI `Select` or equivalent) and ensure `due_date` input uses type `date` and returns `YYYY-MM-DD`.
- Reuse `normalizeDateString` helper in `TaskForm` to coerce dates into ISO `YYYY-MM-DD`.
- Ensure `onSave` contract (`onSave({ title, description, due_date })`) is extended to include `priority`.

## Epic: Filters and Views

- Story: Add `All` / `Today` / `Overdue` filter tabs
  - Acceptance Criteria:
    - UI shows three tabs; user can switch views

- Story: Ensure `All` view shows completed and incomplete tasks
  - Acceptance Criteria:
    - `All` displays both completed and incomplete tasks

- Story: Implement `Today` view to show incomplete tasks due today
  - Acceptance Criteria:
    - `Today` shows only incomplete tasks whose `due_date` equals the current local date

- Story: Ensure `Today` view excludes completed tasks
  - Acceptance Criteria:
    - Completed tasks do not appear in `Today` view

- Story: Implement `Overdue` view to show incomplete overdue tasks
  - Acceptance Criteria:
    - `Overdue` shows only incomplete tasks with `due_date` before current local date

- Story: Ensure `Overdue` view excludes completed tasks
  - Acceptance Criteria:
    - Completed tasks do not appear in `Overdue` view

### Technical Requirements (Filters & Views)

- Add a filter state in the frontend (e.g., `filter: 'all'|'today'|'overdue'`) and compute the visible task list client-side.
- Implement date comparisons using the normalized ISO date (no timezone shift): parse `YYYY-MM-DD` into local date for comparisons.
- Ensure `TaskList` renders filtered results and uses the same data model field names (`due_date`, `priority`).

## Epic: Local Persistence

- Story: Persist tasks to local storage
  - Acceptance Criteria:
    - Created & updated tasks are saved to localStorage and survive page reloads

- Story: Load tasks from local storage on app startup
  - Acceptance Criteria:
    - On startup, tasks are read from localStorage and rendered in the UI

- Story: Persist `dueDate` and `priority` fields to local storage
  - Acceptance Criteria:
    - `due_date` and `priority` fields are present in stored objects and restored on load

### Technical Requirements (Persistence)

- Implement a persistence adapter in the frontend that exposes `loadTasks()` and `saveTasks(tasks)` which use `localStorage` key `todo.tasks` (JSON serialized array).
- Replace or wrap current `fetch`/API calls in `TaskList` with the adapter for MVP; keep an adapter-based API to allow swapping back to backend later.
- Include simple migration: if `todo.tasks` is missing but backend `/api/tasks` exists, optionally import existing tasks into localStorage (manual step documented).

## Epic: UI Display

- Story: Show `dueDate` in task list items
  - Acceptance Criteria:
    - Task list items display `due_date` in a human-friendly format when present

- Story: Show `priority` in task list items
  - Acceptance Criteria:
    - Task list items display `priority` as a label or badge

### Technical Requirements (UI Display)

- Update `packages/frontend/src/TaskList.js` to render `priority` next to each item (use `Chip`/`Badge`) and to use the `formatDueDate` helper to display `due_date`.
- Ensure styles follow `docs/ui-guidelines.md` color recommendations for priority badges (Post‑MVP colors: P1 red, P2 orange, P3 gray).

## Epic: Data Validation

- Story: Prevent saving tasks without a `title`
  - Acceptance Criteria:
    - UI validation prevents saving and shows an error for missing `title`

- Story: Default invalid or missing `priority` to `P3`
  - Acceptance Criteria:
    - Invalid or missing `priority` is saved as `P3`

### Technical Requirements (Validation)

- Implement client-side validation in `TaskForm` (title required). Ensure server/backend validation (if present) also enforces these rules.
- Implement a small validation util module (e.g., `utils/validateTask.js`) used by `TaskForm` and persistence adapter.

## Epic: Overdue Highlighting (Post‑MVP)

- Story: Highlight overdue tasks visually
  - Acceptance Criteria:
    - Overdue & incomplete tasks render with a clear visual highlight (e.g., red outline or background)

### Technical Requirements (Overdue Highlighting)

- Add a style variant to `TaskList` items when task is overdue and incomplete; use CSS-in-JS consistent with existing theme.

## Epic: Sorting Rules (Post‑MVP)

- Story: Implement ordering: overdue → priority → due date → undated
  - Acceptance Criteria:
    - Task ordering follows the specified precedence

### Technical Requirements (Sorting)

- Implement a `sortTasks(tasks)` utility that applies the ordering rules; use stable sort and ensure tasks without `due_date` go last.

## Epic: Priority Badges (Post‑MVP)

- Story: Add colored priority badges for P1/P2/P3
  - Acceptance Criteria:
    - Priority badges use recommended colors (red=P1, orange=P2, gray=P3)

### Technical Requirements (Priority Badges)

- Use MUI `Chip` with color styling; ensure color tokens come from a theme or CSS variables defined in the frontend.

## Epic: UX Polish & Accessibility (Post‑MVP)

- Story: Add filter change transitions and small UI polish
  - Acceptance Criteria:
    - Filter changes include subtle transitions and visual polish improvements

- Story: Improve empty states for each filter view
  - Acceptance Criteria:
    - Each filter view shows a clear, helpful empty state when no tasks match

- Story: Implement accessibility improvements
  - Acceptance Criteria:
    - Accessibility improvements address color contrast, focus indicators, and ARIA labels

### Technical Requirements (UX & Accessibility)

- Add CSS transitions to `TaskList` and tabs; ensure focus outlines and ARIA attributes for interactive elements.
- Run basic accessibility linting (e.g., axe or eslint-plugin-jsx-a11y) during development and address major failures.

---

If you'd like, I can now:
- Open a PR with these docs changes.
- Create a small frontend adapter to implement `loadTasks()`/`saveTasks()` to start replacing API calls with local storage (MVP).
