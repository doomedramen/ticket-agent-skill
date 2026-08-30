---
name: ticket
description: >
  Create, view, or explicitly update a local ticket. Use when the user says
  "/ticket", "create a ticket", "file a ticket", "new ticket", "show ticket",
  "open ticket", or asks to add context to a ticket. A ticket records deferred
  work; filing one does not authorize implementation.
---

# Ticket

Create and view file-based tickets in the current project. A ticket is a
record, not an instruction to start the recorded work.

## When to use

- The user explicitly asks to create, file, show, or open a ticket.
- The user explicitly asks to correct, expand, or add context to a ticket.

Do not create tickets from incidental observations.

## Rules

1. **Repository convention comes first.** Before creating, viewing, or
   transitioning a ticket, look for the project's ticket README, template, or
   equivalent local instructions. Use its directories, filenames, frontmatter,
   statuses, required fields, and archive rules. The fallback format in
   [ticket-format.md](references/ticket-format.md) applies only when the project
   has no ticket convention.
2. **Make the record self-contained.** Start from the user's request. When it
   refers to the active conversation — for example, “this”, “the proposal
   above”, or “document that” — include enough of that discussed material for a
   new reader to understand the ticket. If the user asks for verbatim text,
   include it verbatim. Clearly label unapproved material as a proposal,
   question, or exploration.
3. **Do not invent deferred decisions.** Do not add requirements, acceptance
   criteria, causes, estimates, or design conclusions that were neither stated
   by the user nor already discussed. Do not research or implement the ticket's
   subject merely because it was filed.
4. **Later instructions control the ticket.** A later explicit request to view,
   correct, expand, move, or otherwise update a ticket authorizes that ticket
   operation. It does not authorize implementation unless the user says so.
5. **Keep the response narrow.** Confirm the actual title and path after a
   mutation. Do not offer to begin the deferred work.

## Create

1. Read the project ticket convention when one exists.
2. Resolve references to the current conversation and write the necessary
   context into the ticket. Do not reduce a request such as “document this” to
   an unexplained one-line placeholder.
3. Follow the local template exactly. If there is no local convention, use the
   fallback format in [ticket-format.md](references/ticket-format.md).
4. Check for an obviously similar open ticket only when the local convention
   requires it. Do not block creation on similarity.
5. Confirm the created ticket's real path.

## View or update

Locate the ticket according to the repository's convention. Read only the
ticket and the directly relevant conversation material needed for the user's
requested operation. Preserve the existing format and make only the requested
change.

## Scope boundary

This skill creates and maintains records. It does not implement ticket content,
reprioritize a roadmap, or create related tickets without a separate user
request.
