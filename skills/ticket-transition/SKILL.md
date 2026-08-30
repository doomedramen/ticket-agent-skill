---
name: ticket-transition
description: >
  Move local tickets between project-defined states. Use when the user says
  "/ticket-transition", "close ticket", "start working on ticket", "reopen
  ticket", "move ticket to in-progress", or "mark ticket done". Updates the
  ticket atomically according to the repository's workflow.
---

# Ticket transition

Move a ticket only when the user explicitly requests a state change.

## Rules

1. Read the project's ticket README or equivalent local instructions before the
   transition. Its directories, status values, archive rules, required
   frontmatter, ownership fields, and log format take precedence.
2. If no local workflow exists, use the fallback states `open`, `in-progress`,
   and `closed`; move the file and update its `status` and `updated` fields in
   one operation.
3. Preserve the ticket body except for a transition log required by the local
   workflow or explicitly requested by the user.
4. Never infer a transition from code changes or apparent completion. Never
   transition tickets not explicitly named by the user.
5. For a bulk transition, list the resolved targets and obtain explicit
   confirmation before changing them.

## Process

Locate the ticket using the repository's stable identity, validate that the
requested transition exists in the local workflow, then move the file and
update its required metadata atomically. Confirm the old and new state and the
actual resulting path.
