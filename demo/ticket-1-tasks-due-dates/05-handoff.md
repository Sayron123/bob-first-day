# Handoff — Tasks page: due dates & assignee

## For the client (plain language)
The Tasks page now shows two new columns: **Assignee** (who the task is assigned
to) and **Due Date** (when it's due, formatted as e.g. "Jan 15, 2027").
Any task due **today or within the next 30 days** is highlighted with a soft
amber background across the entire row — making upcoming deadlines easy to spot
at a glance. Tasks that are already overdue are not highlighted.

## For the developer
**Files changed:**
- `src/features/tasks/data/schema.ts` — added `assignee: z.string()` and `dueDate: z.date()` to `taskSchema`
- `src/features/tasks/components/tasks-columns.tsx` — added Assignee and Due Date `ColumnDef<Task>` entries; uses `date-fns/format`
- `src/features/tasks/components/tasks-table.tsx` — added conditional `bg-amber-50 dark:bg-amber-950/20` className on `<TableRow>` via `cn()` + `date-fns` helpers (`startOfDay`, `addDays`, `isBefore`, `isAfter`)
- `src/features/tasks/components/tasks-mutate-drawer.test.tsx` — added `assignee` + `dueDate` to `MOCK_TASK` fixture to satisfy the updated `Task` type

**Checks run:**
- `pnpm build` (`tsc -b && vite build`) — ✅ clean, 3974 modules, no type errors
- `pnpm lint` (eslint) — ✅ clean, no warnings

**Known limits / follow-ups:**
- Mock data generates `dueDate` as a random future date from `faker.date.future()` with a fixed seed; in production the dates would come from a real API.
- The highlight window (30 days) is hard-coded in `tasks-table.tsx`. If it needs to be configurable, extract it to a constant or prop.
- No sorting/filtering by due date was added (out of scope per the ticket).

**Full before/after:** `.bob/tickets/tasks-due-dates/02-map.md`
