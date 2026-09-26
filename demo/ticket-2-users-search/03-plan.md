# Plan — Users Search Broken

## Root cause (corrected)
The Users table wires the search input to `table.getColumn('username')?.setFilterValue()`. TanStack Table's default `includesString` filter already does case-insensitive substring matching on that one column — so typing "jil" does find jill.mosciski. The real bug is simpler: **only the `username` column is searched**. The `fullName` display column has no `accessorKey`, so it can't be targeted by `setFilterValue` at all. The `email` column has an `accessorKey` but is never targeted.

The Tasks table already solves multi-field search with `globalFilter` + a custom `globalFilterFn`. We copy that exact pattern.

---

## Changes (2 files edited, 1 file verified / no edit)

### 1. `src/routes/_authenticated/users/index.tsx`
Replace the `username` URL search param with `filter`. The `useTableUrlState` hook reads the global filter value from `search[globalFilter.key]` (default key: `'filter'`). Without this rename, the search box value is never restored from the URL on page reload.

### 2. `src/features/users/components/users-table.tsx`
- Switch `globalFilter: { enabled: false }` → `{ enabled: true, key: 'filter' }`
- Remove `{ columnId: 'username', searchKey: 'username', type: 'string' }` from `columnFilters` (only status + role remain)
- Destructure `globalFilter` and `onGlobalFilterChange` from the hook result
- Add `globalFilter` to `useReactTable` state; add `onGlobalFilterChange`
- Add `globalFilterFn` that does case-insensitive substring match across `username`, `firstName + ' ' + lastName`, and `email`
- Remove `searchKey='username'` from `<DataTableToolbar>` — the toolbar's `else` branch (`table.setGlobalFilter(...)`) already handles the no-`searchKey` case

### 3. `src/routes/clerk/_authenticated/user-management.tsx` — **no edit needed**
This route uses `UsersTable` and passes `Route.useSearch()` as `search`. It has no `validateSearch` schema, so URL params flow through as-is. The hook reads `search['filter']` dynamically — it will work correctly once `users-table.tsx` is updated. Verified: no change required.

### 4. `src/features/users/components/users-columns.tsx` — **no edit needed**
The `globalFilterFn` accesses `row.original` directly. Status/role `filterFn`s are untouched.

---

## How to verify
- `pnpm lint`
- `pnpm build` (TypeScript + Vite)
- Manual on `/users`: type "Jill" → name rows appear; type "jill_zulauf74@hotmail.com" → email match appears; type "jil" → still works (substring); Status/Role facet filters still work; Reset clears all.

---

## Risk
- Low. Toolbar already has the global-filter `else` branch (toolbar.tsx:47–52). We are not touching it.
- URL shape change: `?username=` → `?filter=`. Any bookmarked URL with `?username=` gracefully degrades to no filter via Zod `.catch('')` on the route schema.
- Clerk route: no schema, no risk.
