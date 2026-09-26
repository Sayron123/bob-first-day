# Handoff — Users Search Broken

## For the client (plain language)

The "Filter users…" search box on the Users page now searches across **username, full name, and email** at the same time. Typing "Jill" will find Jill Mosciski. Pasting an email like jill_zulauf74@hotmail.com will find the matching account. The search is case-insensitive and matches any part of the name or address. The Status and Role filters are unchanged.

## For the developer

**Files changed:**
- `src/routes/_authenticated/users/index.tsx` — renamed URL search param `username` → `filter` to align with the globalFilter key the hook reads
- `src/features/users/components/users-table.tsx` — enabled `globalFilter`, added `globalFilterFn` (case-insensitive substring across `username`, `firstName + lastName`, `email`), removed per-column `username` filter config, removed `searchKey` prop from toolbar

**Files verified / no edit:**
- `src/routes/clerk/_authenticated/user-management.tsx` — also renders `UsersTable`; no schema change needed, works correctly with the updated component
- `src/features/users/components/users-columns.tsx` — untouched; status/role filterFns unchanged

**Checks:** `pnpm lint` clean, `pnpm build` clean (tsc + Vite, 3974 modules, 984 ms).

**Known limits / follow-ups:**
- Any bookmarked URL with `?username=…` will land with no filter applied (graceful degradation via Zod `.catch('')`). No migration needed.
- Phone number is not included in the search — add it to `globalFilterFn` in `users-table.tsx` if needed later.

**Full before/after:** `.bob/tickets/users-search-broken/02-map.md`
