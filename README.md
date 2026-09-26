# Bob First Day 🔎

**An IBM Bob 2.0 custom mode (TicketLens) that gets a developer from a vague client ticket to a safe, verified change in a codebase they've never seen, by making sure they understand it first.**

Built for the IBM Bob 2.0 Hackathon (Sep 25–27, 2026).

---

## The problem

A freelancer or new team member gets a client message like:

> *"Hi! Our team keeps missing deadlines. On the Tasks page, can you show who each task is assigned to and its due date, and highlight anything due in the next 30 days? — Maria"*

The code is unfamiliar: different stack, no comments, deadline pressure. Most AI tools jump straight to editing code the developer doesn't understand, and the developer can't review or explain it.

**Our baseline:** 60 minutes working by hand with no AI → **0 lines changed.** The whole hour went to understanding the code. See [docs/baseline.md](docs/baseline.md).

## The solution: TicketLens mode

One workflow with two stops where the developer must approve before Bob continues. It uses Bob features across the whole task, not just coding:

| # | Step | Bob feature | Developer |
|---|---|---|---|
| 1 | **Read ticket**: client email as screenshot or PDF | Document understanding | drops the file in |
| 2 | **Translate**: "Client means…", scope, ≤3 questions | Custom mode | 🟢 approves |
| 3 | **Map**: 3 parallel read-only subagents (data / UI / conventions) → Mermaid map where every arrow is backed by `file:line` | Subagents, parallel tasks | reads the map |
| 4 | **Plan**: file → change → reason | Custom mode | 🟢 approves |
| 5 | **Change**: small edits, before/after + reason recorded in the map | Agent mode | watches |
| 6 | **Verify**: the project's own build + lint, fix loop until clean | Agent mode | — |
| 7 | **Handoff**: plain-language note for the client + dev summary | — | sends it |

Output per ticket lives in `.bob/tickets/<slug>/` (translation, map, plan, verify log, handoff), so the understanding stays in the repo for the next developer.

## Results

### Ticket 1: "Show assignee + due date, highlight due in 30 days" (Maria, feature request)

| | Manual (no AI) | TicketLens |
|---|---|---|
| Time | 60 min (timeboxed), unfinished | **~22 min**, shipped |
| Files changed | 0 | 4 (schema, columns, table, test fixture) |
| Build + lint | n/a | ✅ both pass on first run |
| Scope clarified before coding | — | Highlight whole row, upcoming only (not overdue) |
| Hidden break caught before coding | — | Test fixture broken by the type change, caught at the plan approval step and fixed in the plan |
| Bobcoins used | — | 2.98 of 40 |

Time varies with ticket size and codebase; these are measured numbers for this ticket, not a promise.

### Ticket 2: "Users search is broken??" (Jordan, bug report)

_In progress._

## Use it on any repo

1. Copy the `.bob/` folder into the root of your project.
2. Open the project in IBM Bob IDE and pick **🔎 TicketLens** from the mode menu.
3. Attach the client's ticket (screenshot, PDF, or text) and say "Start."

## Repo layout

```
.bob/custom_modes.yaml        # TicketLens mode definition
.bob/rules-ticketlens/        # step-by-step rules the mode follows
docs/baseline.md              # manual baseline measurement
demo/                         # demo ticket + demo run outputs
bob_sessions/                 # exported Bob task sessions + summary screenshots
```

## Credits

Demo codebase: [satnaing/shadcn-admin](https://github.com/satnaing/shadcn-admin) (MIT), used as a sample "client" project. This repo does not include its source.
