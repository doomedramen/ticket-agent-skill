---
name: ticket
description: >
  Create a new local ticket or show an existing one. Use when the user says
  "/ticket", "create a ticket", "file a ticket", "new ticket", "show ticket",
  or "open ticket". A ticket is a record, not an instruction — it captures a
  task, bug, or idea for later. Never use automatically.
---

# Ticket

## Overview

Create and display local tickets stored in `./tickets/`. A ticket is a
lightweight, file-based work item — not an instruction, not a spec. It's a
notebook entry with structure.

## When to Use

- User explicitly asks to create/file a ticket
- User asks to see a specific ticket by ID
- User says `/ticket <title>` or `/ticket <id>`

## Never

- Auto-create tickets from casual remarks
- Treat a ticket as an instruction to implement
- Modify a ticket's body during creation (only set frontmatter + structure)
- Create tickets that duplicate existing open ones (check first)

## Core Rules

1. **A ticket is not an instruction.** It's a record. Create it, confirm, move on.
2. **Preserve active execution.** Never derail current work to file a ticket.
3. **Auto-bootstrap.** Create `tickets/{open,in-progress,closed}/` if missing.
4. **Minimal confirmation.** `✓ Ticket #001 created: <title>` — then resume.

## Create a Ticket

Triggered by: `/ticket <title>`, "create ticket", "file a ticket", "new ticket"

### Process

1. **Bootstrap** — if `tickets/` doesn't exist, create `tickets/open/`, `tickets/in-progress/`, `tickets/closed/`
2. **Find next ID** — scan all three dirs for highest numeric ID, increment. First ticket = `001`.
3. **Slugify title** — lowercase, spaces to hyphens, strip special chars, truncate at 60 chars
4. **Determine priority** — `medium` default. Infer from title keywords:
   - `urgent`, `critical`, `blocker`, `P0` → `high`
   - `low`, `minor`, `nice-to-have`, `P2` → `low`
   - Everything else → `medium`
5. **Write file** to `tickets/open/{id}-{slug}.md`

### Ticket Template

```markdown
---
id: "{id}"
title: "{title}"
status: open
priority: {priority}
created: "{ISO 8601 timestamp}"
updated: "{ISO 8601 timestamp}"
tags: []
---

## Problem

{user's description or a brief summary}

## Acceptance Criteria

- [ ] {criterion inferred from title or left generic}
```

### Confirmation

```
✓ Ticket #{id} created: {title}
  Priority: {priority}
  File: tickets/open/{id}-{slug}.md
```

Then resume immediately. Do not elaborate, suggest next steps, or evaluate.

## Show a Ticket

Triggered by: `/ticket 001`, "show ticket 001", "open ticket 001"

### Process

1. Search all three dirs for file starting with `{id}-`
2. If not found: `✗ Ticket #{id} not found`
3. If found: display frontmatter as compact summary + full body

### Output Format

```
Ticket #{id}: {title}
Status: {status} | Priority: {priority}
Created: {created} | Updated: {updated}
Tags: {tags}

{full markdown body}
```

## Edge Cases

- **Duplicate title check**: before creating, grep `tickets/open/` for similar titles. If found, warn: `⚠ Similar open ticket exists: #{id} — {title}. Create anyway?`
- **ID collision**: impossible if scanning all dirs, but if somehow duplicate, skip to next number
- **Missing dirs**: auto-create, don't error
- **Empty ID search**: `tickets not found` — don't create one

## What This Does Not Do

- List all tickets → use `/tickets`
- Transition ticket state → use `/ticket-transition`
- Implement, assign, prioritize in bulk, or manage a backlog
- Sync with external issue trackers
- Create branches, PRs, or commits from tickets
