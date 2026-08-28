# ticket-agent-skill

Local, file-based ticket management for Claude. Lightweight issues living in your project directory.

## What It Does

Three skills that give you a minimal ticket board without leaving the terminal:

| Skill | Command | Purpose |
|-------|---------|---------|
| `ticket` | `/ticket` | Create a ticket, show a ticket |
| `tickets` | `/tickets` | List, filter, search the board |
| `ticket-transition` | `/ticket-transition` | Move tickets between states |

## Install

```
npx skills add doomedramen/ticket-agent-skill --skill ticket
npx skills add doomedramen/ticket-agent-skill --skill tickets
npx skills add doomedramen/ticket-agent-skill --skill ticket-transition
```

Or install all at once:

```
npx skills add doomedramen/ticket-agent-skill
```

## Ticket Storage

```
./tickets/
├── open/
│   ├── 001-fix-login-bug.md
│   └── 002-add-dark-mode.md
├── in-progress/
│   └── 003-db-migration.md
└── closed/
    └── 004-setup-ci.md
```

The directory structure is auto-created on first use.

## Usage

### Create a ticket

```
> /ticket Fix login timeout on session refresh

✓ Ticket #001 created: Fix login timeout on session refresh
  Priority: medium
  File: tickets/open/001-fix-login-timeout-on-session-refresh.md
```

### Show a ticket

```
> /ticket 001

Ticket #001: Fix login timeout on session refresh
Status: open | Priority: medium
Created: 2026-08-28 | Tags: []

## Problem
Users getting logged out after 30min despite valid tokens...
```

### List tickets

```
> /tickets

Tickets (open): 2

  #   Title                          Priority   Created
  001 Fix login timeout               medium     2026-08-28
  002 Add dark mode toggle            low        2026-08-27
```

### Filter tickets

```
> /tickets --priority high --all
> /tickets --tag auth
> /tickets --status closed
```

### Board view

```
> /tickets --board

Open (2)                 In Progress (1)         Closed (1)
─────────────            ────────────────         ──────────
#001 Fix login           #003 DB migration        #004 Setup CI
#002 Dark mode
```

### Transition tickets

```
> /ticket-transition 001 in-progress

✓ Ticket #001 Fix login timeout
  open → in-progress
  File: tickets/in-progress/001-fix-login-timeout-on-session-refresh.md

> /ticket-transition 001 closed --reason "Fixed in abc123"

✓ Ticket #001 Fix login timeout
  in-progress → closed
  Reason: Fixed in abc123
  File: tickets/closed/001-fix-login-timeout-on-session-refresh.md
```

## Ticket Format

Each ticket is a markdown file with YAML frontmatter:

```yaml
---
id: "001"
title: "Fix login timeout on session refresh"
status: open
priority: medium
created: "2026-08-28T10:00:00Z"
updated: "2026-08-28T10:00:00Z"
tags: []
---

## Problem

Description of the issue or task.

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
```

See `skills/ticket/references/ticket-format.md` for the full specification.

## Design Principles

- **A ticket is not an instruction.** It's a record — like a note with structure.
- **File-based.** No database, no sync, no external services. Just markdown.
- **Auto-bootstrapped.** Directory structure created on first use.
- **Minimal.** Three states, three skills, zero config.
- **Append-only bodies.** Ticket content is never rewritten, only appended to (transition logs).

## What It Doesn't Do

- Sync with GitHub Issues, Jira, Linear, or any external tracker
- Auto-create tickets from code changes or CI failures
- Manage sprints, milestones, or roadmaps
- Assign tickets to people
- Generate reports or analytics
- Delete tickets (ever)

## License

MIT
