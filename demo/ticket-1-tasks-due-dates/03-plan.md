# Plan — Tasks page: due dates & assignee

## Changes (4 files, no new files, no new dependencies)

1. `src/features/tasks/data/schema.ts` — Add `assignee: z.string()` and
   `dueDate: z.date()` to the Zod schema so the inferred `Task` type exposes
   both fields to the rest of the app.
   *(Scope: "Add assignee and dueDate fields to the Task data model")*

2. `src/features/tasks/components/tasks-columns.tsx` — Add two new
   `ColumnDef<Task>` entries before the `actions` column:
   - **Assignee** — plain text cell rendering `row.original.assignee`
   - **Due Date** — formats `row.original.dueDate` with `date-fns`
     `format(date, 'MMM d, yyyy')`. Already-available `date-fns` library,
     no new dependency.
   *(Scope: "Add two new columns: Assignee and Due Date")*

3. `src/features/tasks/components/tasks-table.tsx` — Add a conditional
   `className` to the `<TableRow>` using `cn()`. Import `isAfter`, `isBefore`,
   `addDays`, `startOfDay` from `date-fns`. Highlight logic:
   `today <= dueDate <= today+30` → add `'bg-amber-50 dark:bg-amber-950/20'`
   (a soft amber tint, consistent with shadcn's warning palette).
   *(Scope: "Highlight entire row when dueDate is today to +30 days, not overdue")*

4. `src/features/tasks/components/tasks-mutate-drawer.test.tsx` — Add
   `assignee` and `dueDate` fields to the `MOCK_TASK` fixture (line 11–17)
   so that `as const satisfies Task` continues to type-check after the schema
   extension. No test logic changes.
   *(Scope: fix type break caused by item 1)*

## Risk notes
- **`taskSchema.parse(row.original)` in `data-table-row-actions.tsx:29`** — safe
  because `row.original` is sourced from `tasks.ts` mock data, which already
  generates `assignee` and `dueDate`; adding required fields to the schema only
  makes the parse stricter against data that already satisfies it.
- All other consumers of `type Task` (`tasks-provider.tsx`, `tasks-table.tsx`,
  `tasks-columns.tsx`, `data-table-bulk-actions.tsx`) use it as a type annotation
  only; adding fields is purely additive for them.
- `tasks-mutate-drawer.test.tsx` is the single fixture that would break without
  item 4 above; patching it is the complete fix.
- Row highlight uses only Tailwind classes already pulled in by the project.

## How to verify
- `pnpm build` — runs `tsc -b` (type-check) + Vite build; must pass clean.
- `pnpm lint` — ESLint must pass with no new errors.
- Manual: open the Tasks page and confirm the two new columns appear and that
  some rows show an amber background (those with due dates in the next 30 days).
