# Translation — Tasks page: due dates & assignee

## Client said
> "On the Tasks page, can you show who each task is assigned to and its due date,
> and highlight anything due in the next 30 days?"

## Client means (technical)
Add an **Assignee** column and a **Due Date** column to the Tasks page data table;
rows whose due date falls within the next 30 calendar days should be visually
highlighted (e.g. a badge or row/cell style).

## In scope
- Add `assignee` and `dueDate` fields to the Task data model (or confirm they already exist).
- Add mock data values for both fields if not already populated.
- Add two new columns to the TanStack Table column definitions: Assignee and Due Date.
- Highlight the **entire row** when `dueDate` is between today (inclusive) and today + 30 days (inclusive).
- Overdue tasks (dueDate < today) must NOT be highlighted.

## Out of scope
- Editing or reassigning tasks from the UI.
- Server/API integration (this is a mock-data project).
- Sorting or filtering by due date (not requested).
- Any other page or feature.

## Assumptions
- The Tasks page already exists in the codebase.
- The project uses TanStack Table (common in shadcn-admin starters) for the data table.
- "30 days" means `dueDate <= today + 30 days` (inclusive).
- Highlight style will follow existing badge/colour conventions already in the project.

## Questions
_(none blocking)_
