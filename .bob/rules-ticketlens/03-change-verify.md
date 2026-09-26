# Step 5 — Change / Step 6 — Verify

## Change
- Only touch files listed in the approved plan. If you discover another file
  must change, STOP and ask before touching it.
- Follow the conventions found in Step 3 (same patterns, helpers, styling).
  No new dependencies unless the plan approved them.
- Keep edits small. No drive-by refactors, renames or formatting of
  unrelated code.
- After each file, append to `02-map.md` under `## Changes`:

```
### `path/to/file.tsx`
**Before** (lines X–Y)
```tsx
<the original snippet, short>
```
**After**
```tsx
<the new snippet, short>
```
**Why:** one or two sentences tied to the ticket.
```

- Then update `## Flow — after` in the map.

## Verify
- Run the project's own checks found in Step 3 (e.g. build, lint, typecheck,
  tests). Use the package manager the project already uses (check the lock
  file).
- If a check fails: read the error, fix the cause (not by disabling rules or
  adding `any`/ignore comments), re-run. Repeat until clean, max 3 attempts;
  then stop and explain what is blocking.
- Only report "done" when every check passes. Paste the final pass lines
  (short) into `.bob/tickets/<slug>/04-verify.md`.
- Tell the developer how to see the result in the running app
  (URL/route and what to look for).
