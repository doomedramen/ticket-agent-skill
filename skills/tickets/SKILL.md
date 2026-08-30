---
name: tickets
description: >
  List, filter, and search local tickets. Use when the user says "/tickets",
  "list tickets", "show tickets", "show open tickets", "board", "ticket board",
  or "what tickets are there". Read-only — never modifies tickets.
---

# Tickets

List a project's file-based tickets without changing them.

## Rules

1. Read the project's ticket README or equivalent local instructions first when
   they exist. Use its status directories, archive layout, filename identity,
   and frontmatter fields; do not assume numeric IDs or `closed/`.
2. If no local workflow exists, use the fallback layout documented by the
   `ticket` skill: `tickets/{open,in-progress,closed}/`.
3. This skill is read-only. Do not create, transition, edit, or implement a
   ticket.
4. Default to the project's open or equivalent actionable state. Keep the
   listing compact and do not editorialize.

## Output

Show each ticket's stable identifier (the local filename when no ID exists),
title, status, priority when available, and creation date when available.
Honor user filters only when the local ticket metadata supports them. For a
board request, group tickets by the project's actual statuses.

If no tickets match, say `No tickets found.`
