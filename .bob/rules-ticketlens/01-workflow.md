# TicketLens workflow (follow in order)

Every ticket gets its own folder: `.bob/tickets/<ticket-slug>/`
(slug = 2–4 lowercase words from the ticket, e.g. `tasks-due-dates`).
Create a todo list with the 7 steps at the start and tick them off as you go.

## Step 1 — Read the ticket
- The ticket may be an image, PDF, or text. Read it fully (use document/image
  understanding for screenshots and PDFs).
- Extract: who asked, what they see today, what they want, any deadline.
- Do NOT open source files beyond a quick look at the project structure yet.

## Step 2 — Translate  🟢 APPROVAL GATE
Write `.bob/tickets/<slug>/01-translation.md` using this shape:

```
## Client said
> <short quote>

## Client means (technical)
<one or two sentences in developer language>

## In scope
- ...
## Out of scope
- ...
## Assumptions
- ...
## Questions (max 3, only if truly blocking)
1. ...
```

Then reply in chat with ONLY the "Client means" sentence, scope bullets and
questions, and ask: **"Are we solving the same problem? (yes / correct me)"**
STOP. Do not continue until the developer says yes. If corrected, rewrite the
file and ask again.

## Step 3 — Map (parallel subagents)
Spawn up to 3 read-only **Explore** subagents IN PARALLEL, each with a
focused brief that includes the approved "Client means" sentence:
1. **Data** — where the data for this feature comes from: types/schemas,
   mock data or API calls, and how it flows into the UI.
2. **UI** — the route/page, components, and table/column/form definitions
   that render this feature.
3. **Conventions** — how similar features are already built here (patterns
   to copy, helpers, styling approach), plus the project's build/lint/test
   commands from package.json (or equivalent).

Each subagent must return findings as `path:line — fact` lines only.
Merge them into `.bob/tickets/<slug>/02-map.md` following
`02-map-format.md`. Then give the developer a 3-line summary in chat.

## Step 4 — Plan  🟢 APPROVAL GATE
Write `.bob/tickets/<slug>/03-plan.md`: a numbered list, one item per file:

```
1. `path/to/file.tsx` — what changes — why (links to scope bullet)
```

Include: new files (if any), risk notes, and how the result will be checked.
Reply with the plan and ask: **"Approve this plan? (yes / change X)"**
STOP. No source edits before a clear yes.

## Step 5 — Change
Follow `03-change-verify.md`.

## Step 6 — Verify
Follow `03-change-verify.md`.

## Step 7 — Handoff
Follow `04-handoff.md`.
