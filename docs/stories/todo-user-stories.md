# User Stories & Acceptance Tests - Todo App Upgrade

## Epic: Task Organization Improvements

### Story 1 — Add Due Date to Tasks
- As a user, I want to set an optional due date when creating or editing a task so I can track deadlines.

Acceptance Criteria / Tests:
- Given the task form is open, when I enter a valid ISO `YYYY-MM-DD` date in the due date field and save, then the saved task includes `dueDate` with the same value.
- Given the task form is open, when I enter an invalid date and save, then the task is saved without a `dueDate` and no error blocks saving.
- Given a task with `dueDate` set, when I reload the page, then the task and its `dueDate` persist (local storage).

---

### Story 2 — Add Priority to Tasks
- As a user, I want to set a priority on a task (P1, P2, P3) so I can surface critical work.

Acceptance Criteria / Tests:
- Given the task form is open, when I select `P1`, `P2`, or `P3` and save, then the saved task includes `priority` with that value.
- Given no priority is selected, when I save a task, then `priority` defaults to `P3`.
- Given a task with `priority` set, when I reload the page, then the task and its `priority` persist (local storage).

---

### Story 3 — Filters: All, Today, Overdue
- As a user, I want filter tabs for `All`, `Today`, and `Overdue` so I can quickly view relevant tasks.

Acceptance Criteria / Tests:
- Given tasks exist with various due dates and completion statuses, when I select `All`, then I see both completed and incomplete tasks.
- Given tasks exist, when I select `Today`, then I see only incomplete tasks whose `dueDate` is equal to the current date.
- Given tasks exist, when I select `Overdue`, then I see only incomplete tasks whose `dueDate` is before the current date.

---

### Story 4 — Local Persistence Only
- As a user, I expect data to persist between reloads without requiring a backend.

Acceptance Criteria / Tests:
- Given I create or update tasks, when I reload the browser, then all tasks and their fields (`title`, `dueDate`, `priority`, `completed`) persist using local storage.

---

### Story 5 — Display & Minimal UI Changes
- As a user, I want to see `dueDate` and `priority` in task list items and be able to edit them from the existing UI.

Acceptance Criteria / Tests:
- Given a task with `dueDate` and `priority`, when I view the task list, then each item shows the title, due date (if present), and priority.
- Given I click edit on a task, when I change its `dueDate` or `priority` and save, then the list reflects the updated values.

---

### Story 6 — Data Validation Rules (MVP)
- As a developer, I want clear validation rules so the app behaves consistently.

Acceptance Criteria / Tests:
- `title` is required — creating a task without `title` should be prevented with a UI validation message.
- `priority` must be one of `P1|P2|P3` — invalid values should be coerced to `P3` on save.
- `dueDate` must be ISO `YYYY-MM-DD` — invalid values are ignored and treated as absent.

---

## Post-MVP Stories (for later sprints)

### Story A — Overdue Visual Highlighting
- As a user, I want overdue tasks to be visually highlighted so they stand out.

Acceptance Criteria / Tests:
- Given a task is overdue and incomplete, when I view the list, then the task is rendered with a visual highlight (e.g., red text or background) distinct from normal tasks.

---

### Story B — Sorting Rules
- As a user, I want tasks sorted so the most urgent appear first.

Acceptance Criteria / Tests:
- Given multiple tasks, when viewing a list, then tasks are ordered: overdue tasks first → within that group order by `priority` (P1 → P3) → then by `dueDate` ascending → tasks without `dueDate` last.

---

### Story C — Priority Badges
- As a user, I want colored badges for priority to identify urgency at a glance.

Acceptance Criteria / Tests:
- Given a task with `priority` P1/P2/P3, when viewing the list, then the item shows a color badge (recommended: red=P1, orange=P2, gray=P3).

---

## Notes
- These stories and tests are derived from the PRD (`docs/prd-todo.md`) and the original meeting and Slack artifacts.
- Once you approve, I can: generate Jest tests for the front-end components, implement the features, or open a PR containing these docs.
