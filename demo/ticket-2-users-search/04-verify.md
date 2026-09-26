# Verify — Users Search Broken

## Checks run

### pnpm lint
```
(no output — clean)
```

### pnpm build
```
✓ 3974 modules transformed.
✓ built in 984ms
```
TypeScript (`tsc -b`) and Vite bundle both passed with zero errors or warnings.

## How to see the result

Open the app and navigate to **`/users`**.

1. Type **"Jill"** in the "Filter users…" box → rows containing "Jill" in the **Name** column appear
2. Type **"jill_zulauf74@hotmail.com"** → the matching row appears via **email** match
3. Type **"jil"** → still works (substring, case-insensitive)
4. Clear the box, use the **Status** or **Role** facet dropdowns → still filter correctly
5. Click **Reset** → clears all filters
6. The same table at **`/clerk/user-management`** (Clerk variant) works identically — no code change was needed there
