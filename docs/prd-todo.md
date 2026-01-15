# Product Requirements Document (PRD) - Todo App: Due Dates, Priorities, and Filters

## 1. Overview

We are upgrading the basic TODO app to support due dates, priorities, and simple filters so users can better organize and surface urgent work without introducing backend complexity. The goal is a small, teachable MVP that improves task organization while keeping storage local and implementation simple.

---

## 2. MVP Scope

- Add `dueDate` field for tasks (optional). Format: ISO `YYYY-MM-DD`. Invalid date values should be treated as absent.
- Add `priority` field as an enum with values `P1`, `P2`, `P3` (default `P3`).
- Update the data model validation:
  - `title`: required
  - `priority`: must be one of `P1|P2|P3`, default `P3`
  - `dueDate`: optional ISO `YYYY-MM-DD`
- Add three filter views/tabs: `All`, `Today`, `Overdue`.
  - `All` shows completed and incomplete tasks.
  - `Today` and `Overdue` show only incomplete tasks.
- Keep storage local (no backend or external persistence changes).
- UI: Minimal changes to `TaskForm` and `TaskList` to capture and display the new fields. Use simple, clear labels and keep layout lightweight.

Acceptance criteria (MVP):
- A user can create a task with title, optional due date, and priority.
- Invalid `dueDate` input is ignored and the task is saved without a due date.
- The three filter views work as specified and reflect completion status rules.
- The app stores tasks locally and persists across page reloads.

---

## 3. Post-MVP Scope

- Visually highlight overdue tasks (e.g., red background or badge) to make them stand out.
- Apply sorting: overdue tasks first → then by priority (P1 → P3) → then due date ascending → tasks without a due date last.
- Add color-coded priority badges: red for `P1`, orange for `P2`, gray for `P3`.
- UX polish: transitions/animations when tasks move between filters, clearer empty states for the three views, and accessibility improvements.

---

## 4. Out of Scope

- Notifications or reminders.
- Recurring tasks or schedule patterns.
- Multi-user or server-backed task syncing.
- Keyboard-only navigation/advanced accessibility features for the initial MVP.
- External storage or backend changes; keep persistence local.
