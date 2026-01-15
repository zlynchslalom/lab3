Epic: Task Data Model
  - Story: Add dueDate field to task model
    - Acceptance: `dueDate` accepts ISO `YYYY-MM-DD` or is absent; invalid values are ignored
  - Story: Add priority enum to task model
    - Acceptance: `priority` is one of `P1|P2|P3` with default `P3`
  - Story: Enforce required title field
    - Acceptance: Creating a task without `title` is prevented by UI validation
  - Story: Validate dueDate ISO format and ignore invalid values
    - Acceptance: Invalid `dueDate` input is treated as absent and does not block saving

Epic: Task Creation and Editing
  - Story: Add due date input to TaskForm
    - Acceptance: Users can enter a valid ISO `YYYY-MM-DD` and saved tasks include `dueDate`
  - Story: Add priority selector to TaskForm
    - Acceptance: Users can select `P1`, `P2`, or `P3`; missing selection defaults to `P3`
  - Story: Allow editing dueDate and priority on existing tasks
    - Acceptance: Edits to `dueDate` and `priority` persist after save and reload

Epic: Filters and Views
  - Story: Add All / Today / Overdue filter tabs
    - Acceptance: Tabs are present and selectable in the UI
  - Story: Ensure `All` view shows completed and incomplete tasks
    - Acceptance: `All` displays both completed and incomplete tasks
  - Story: Implement Today view to show incomplete tasks due today
    - Acceptance: `Today` shows only incomplete tasks whose `dueDate` equals current date
  - Story: Ensure `Today` view excludes completed tasks
    - Acceptance: Completed tasks do not appear in `Today` view
  - Story: Implement Overdue view to show incomplete overdue tasks
    - Acceptance: `Overdue` shows only incomplete tasks with `dueDate` before current date
  - Story: Ensure `Overdue` view excludes completed tasks
    - Acceptance: Completed tasks do not appear in `Overdue` view

Epic: Local Persistence
  - Story: Persist tasks to local storage
    - Acceptance: Created/updated tasks are stored in local storage and survive reloads
  - Story: Load tasks from local storage on app startup
    - Acceptance: On startup, tasks are read from local storage and rendered
  - Story: Persist `dueDate` and `priority` fields to local storage
    - Acceptance: `dueDate` and `priority` fields persist in local storage and reload correctly

Epic: UI Display
  - Story: Show dueDate in task list items
    - Acceptance: Task list items display `dueDate` when present
  - Story: Show priority in task list items
    - Acceptance: Task list items display `priority` badge or label

Epic: Data Validation
  - Story: Prevent saving tasks without a title
    - Acceptance: UI validation prevents saving and shows an error for missing `title`
  - Story: Default invalid or missing priority to P3
    - Acceptance: Invalid or missing `priority` is saved as `P3`

Epic: Overdue Highlighting (Post-MVP)
  - Story: Highlight overdue tasks visually
    - Acceptance: In Post‑MVP, overdue & incomplete tasks render with a clear visual highlight

Epic: Sorting Rules (Post-MVP)
  - Story: Implement ordering: overdue → priority → due date → undated
    - Acceptance: Tasks sort: overdue first, then by priority (P1→P3), then by due date ascending, then undated

Epic: Priority Badges (Post-MVP)
  - Story: Add colored priority badges for P1/P2/P3
    - Acceptance: Priority badges use recommended colors (red=P1, orange=P2, gray=P3)

Epic: UX Polish & Accessibility (Post-MVP)
  - Story: Add filter change transitions and small UI polish
    - Acceptance: Filter changes include subtle transitions and visual polish improvements
  - Story: Improve empty states for each filter view
    - Acceptance: Each filter view shows a clear, helpful empty state when no tasks match
  - Story: Implement accessibility improvements
    - Acceptance: Accessibility improvements address color contrast, focus indicators, and ARIA labels (Post‑MVP)
