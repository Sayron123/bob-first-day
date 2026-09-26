# Database / backend tickets (extra care)

Applies when the ticket, the map, or the plan touches any of: SQL or ORM
models, migrations, database schemas, seed data, stored data, API routes
that write data, or environment/connection config.

## In Step 2 (Translate)
- Say explicitly: "This ticket changes stored data or the database schema."
- Ask whether existing records must be kept, migrated, or backfilled.

## In Step 3 (Map)
- The Data subagent traces all the way down: UI → API/route → service →
  model/ORM → table/migration. Every hop needs `path:line` evidence.
- List every place that reads or writes the affected table/column.

## In Step 4 (Plan)  🟢 extra approval
Add a **Database impact** section:
- Schema change (new/renamed/removed column, index, constraint).
- Migration file to add (never edit an already-applied migration).
- Effect on existing rows (default value, backfill, nullability).
- Rollback: how to undo the migration.
Ask for approval of the database part **separately** from the code part.

## Hard rules
- Never run destructive commands (DROP, TRUNCATE, DELETE without WHERE,
  `migrate reset`, `db push --force-reset`) without the developer typing
  explicit approval for that exact command.
- Never run migrations against a production database. Local/dev only.
- Never print, log or commit connection strings, passwords or keys.
