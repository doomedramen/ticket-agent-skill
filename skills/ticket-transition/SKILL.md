---
name: ticket-transition
description: >
  Move tickets between states: open, in-progress, closed. Use when the user
  says "/ticket-transition", "close ticket", "start working on ticket",
  "reopen ticket", "move ticket to in-progress", or "mark ticket done".
  Handles file move + frontmatter update atomically.
---

# Ticket Transition

## Overview

Move a ticket file between `tickets/open/`, `tickets/in-progress/`, and
`tickets/closed/` while updating its frontmatter status and timestamp.

## When to Use

- User wants to start working on a ticket
- User wants to close/complete a ticket
- User wants to reopen a closed ticket
- User says "close #001", "start on 003", "reopen 005"

## Never

- Delete tickets
- Modify ticket body content
- Transition a ticket that doesn't exist
- Auto-transition based on code changes or assumptions
- Transition tickets not explicitly mentioned

## Core Rules

1. **Atomic operation.** Move file + update frontmatter in one step.
2. **Explicit only.** Never infer transition intent.
3. **Log the reason.** When provided, append a timestamped note.
4. **Resume immediately.** Confirm, don't editorialize.

## Valid Transitions

```
open ──────────→ in-progress ──────────→ closed
  ↑                  │                      │
  │                  ↓                      │
  └──────────────── ← ───────────────────── ┘
  (reopen)           (park/stall)          (reopen)
```

| From | To | Trigger phrases |
|------|----|-----------------|
| `open` | `in-progress` | "start on", "begin", "working on", "start ticket" |
| `in-progress` | `closed` | "close", "done", "finished", "complete", "mark done" |
| `closed` | `open` | "reopen", "re-open", "bring back" |
| `in-progress` | `open` | "park", "backlog", "not now" |

## Process

1. **Find ticket** — search all three dirs for `{id}-*.md`. If not found: `✗ Ticket #{id} not found`
2. **Validate transition** — check current status matches expected source (or allow any→any with explicit target)
3. **Read file** — parse frontmatter, preserve body
4. **Update frontmatter**:
   - Set `status` to target state
   - Set `updated` to current ISO 8601 timestamp
5. **If reason provided** — append to body:
   ```markdown
   ---

   **{status change}** — {ISO 8601 timestamp}
   {reason}
   ```
6. **Move file** — from source dir to target dir. Filename stays the same (ID doesn't change).
7. **Confirm**

## Confirmation

```
✓ Ticket #{id} {title}
  {old_status} → {new_status}
  File: tickets/{new_status}/{filename}
```

With reason:

```
✓ Ticket #{id} {title}
  open → closed
  Reason: Fixed in abc123
  File: tickets/closed/001-fix-login-bug.md
```

## Special: Close with reason

When user provides a reason (e.g., "close #001 — fixed in PR #42"), append
a transition log to the ticket body before moving:

```markdown
## Problem

Users getting 401...

## Acceptance Criteria

- [x] Token refresh works

---

**closed** — 2026-08-28T15:30:00Z
Fixed in PR #42
```

## Bulk Transition

If user says "close all in-progress tickets" or "move everything to open":

1. List affected tickets
2. Confirm: `⚠ This will transition {n} tickets. Proceed?`
3. Only proceed on explicit confirmation

## Edge Cases

- **Same state**: `✗ Ticket #{id} is already {status}` — don't error, just inform
- **File write fails**: `✗ Failed to write ticket: {reason}` — don't move the file
- **File move fails**: rollback frontmatter change, report error
- **Missing target dir**: auto-create it

## What This Does Not Do

- Create tickets → use `/ticket`
- List/filter tickets → use `/tickets`
- Delete tickets (ever)
- Auto-detect completion from code changes
- Modify ticket content beyond frontmatter + transition log
