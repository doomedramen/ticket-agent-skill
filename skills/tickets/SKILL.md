---
name: tickets
description: >
  List, filter, and search local tickets. Use when the user says "/tickets",
  "list tickets", "show tickets", "show open tickets", "board", "ticket board",
  or "what tickets are there". Read-only — never modifies tickets.
---

# Tickets

## Overview

List and filter tickets across `./tickets/{open,in-progress,closed}/`. Read-only
view into the ticket board.

## When to Use

- User asks to see their tickets
- User wants a board overview
- User filters by status, priority, or tag

## Never

- Modify, delete, or transition tickets
- Create new tickets
- Implement or act on ticket content
- Auto-trigger on unrelated work

## Core Rules

1. **Read-only.** This skill never writes or moves files.
2. **Default to open.** No filter = show open tickets only.
3. **Compact output.** Table format, one line per ticket.
4. **Resume immediately.** Show results, don't editorialize.

## Default Behavior

`/tickets` with no flags → show all `open` tickets as a compact table.

## Flags

| Flag | Effect | Default |
|------|--------|---------|
| `--all` | Show all statuses | open only |
| `--status <status>` | Filter by status | open |
| `--priority <priority>` | Filter by priority | all |
| `--tag <tag>` | Filter by tag | all |
| `--verbose` / `-v` | Show full ticket body | compact |

## Process

1. **Scan** `tickets/{open,in-progress,closed}/` for `*.md` files
2. **Parse** frontmatter of each file
3. **Filter** by provided flags
4. **Sort** by priority (high→medium→low), then by created date (newest first)
5. **Display** results

## Compact Output (default)

```
Tickets (open): 3

  #   Title                          Priority   Created
  001 Fix login bug                   high       2026-08-28
  002 Add dark mode toggle            medium     2026-08-27
  003 Update API docs                 low        2026-08-26
```

## Verbose Output (`-v`)

For each ticket, show full frontmatter summary + body:

```
#001 Fix login bug
  Status: open | Priority: high
  Created: 2026-08-28 | Tags: auth
  ─────────────────────────────────
  ## Problem
  Users getting 401 on valid tokens...
```

## Filtered Output

```
$ /tickets --status in-progress

Tickets (in-progress): 1

  #   Title                          Priority   Created
  003 Database migration v2           high       2026-08-25

$ /tickets --priority high

Tickets (open + high): 2

  #   Title                          Status        Created
  001 Fix login bug                   open          2026-08-28
  003 Database migration v2           in-progress   2026-08-25
```

## Empty State

```
No tickets found.
```

Don't suggest creating one. Don't explain. Just the message.

## Board Summary

When user says "board" or "ticket board", show a grouped view:

```
Open (3)                 In Progress (1)         Closed (2)
─────────────            ────────────────         ──────────
#001 Fix login bug       #003 DB migration v2     #005 Setup CI
#002 Dark mode toggle    ────────────────         #006 Init repo
#007 Update docs
```

## What This Does Not Do

- Create tickets → use `/ticket`
- Transition tickets → use `/ticket-transition`
- Implement, assign, or manage tickets
- Sync with external trackers
