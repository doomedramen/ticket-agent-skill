---
name: ticket
description: >
  Create a new local ticket or show an existing one. Use when the user says
  "/ticket", "create a ticket", "file a ticket", "new ticket", "show ticket",
  or "open ticket". A ticket is a record, not an instruction — it captures a
  task, bug, or idea for later. Never use automatically. Filing or showing a
  ticket is a filing action only — never analyse, diagnose, evaluate, or offer
  to implement its contents.
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
- Create tickets that duplicate existing open ones (note it, don't block)

## Hard Prohibitions

Filing a ticket is a **write operation, not a request for help**. The user is
deliberately deferring this work. Engaging with the content defeats the point.

After creating a ticket you MUST NOT, in the same turn or any later turn:

- Read, grep, or open any file to "check" the ticket's subject
- Diagnose, root-cause, or speculate about the problem
- Propose a fix, approach, design, or implementation plan
- Estimate effort, difficulty, risk, or complexity
- Judge the idea (agree, disagree, "good catch", "makes sense", "that's tricky")
- Offer to start work ("want me to fix this now?", "shall I take a look?")
- Suggest related tickets, refactors, or follow-ups
- Ask clarifying questions about the ticket's substance

The only permitted response is the confirmation block. Then stop.

### Banned Phrasings

Any of these means you have failed:

- "This is likely caused by..."
- "You could fix this by..."
- "Good idea — this would..."
- "Want me to implement this?"
- "Note that this may conflict with..."
- "While filing this, I noticed..."

### Rationalizations To Reject

| Thought | Reality |
|---------|---------|
| "The fix is obvious, one line" | Not asked. File it. Stop. |
| "Being helpful means adding context" | Helpful = not derailing them. |
| "I already know the answer" | Answer belongs in a later session. |
| "I'll just note one caveat" | A caveat is engagement. No. |
| "They'd want to know this" | They chose to defer. Respect it. |
| "It's a trivial ticket, no harm" | Same rule. Every time. |

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

{user's words, verbatim. If they gave only a title, repeat the title.}
```

**Body rule:** transcribe, do not author. Copy what the user said. Do not
summarise, reword, expand, structure, or add sections. Do not invent
acceptance criteria — writing them requires deciding how the work should be
done, which is exactly the thinking being deferred. If the user dictated
acceptance criteria, copy them under an `## Acceptance Criteria` heading;
otherwise omit the heading entirely.

### Confirmation

```
✓ Ticket #{id} created: {title}
  Priority: {priority}
  File: tickets/open/{id}-{slug}.md
```

That block is the entire response. Nothing before it, nothing after it.

If the user was mid-task when they filed the ticket, resume that task at the
exact point it was interrupted — the ticket contributes nothing to it.

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

Display only. The Hard Prohibitions apply identically to showing a ticket:
no commentary, no assessment, no offer to work on it. If the user wants it
actioned they will say so explicitly, or use `/ticket-transition`.

## Edge Cases

- **Duplicate title check**: before creating, grep `tickets/open/` for similar titles. If found, still create the ticket, then append one line to the confirmation: `⚠ Similar open ticket: #{id} — {title}`. Do not ask permission, do not compare the two, do not offer to merge them.
- **ID collision**: impossible if scanning all dirs, but if somehow duplicate, skip to next number
- **Missing dirs**: auto-create, don't error
- **Empty ID search**: `tickets not found` — don't create one

## What This Does Not Do

- List all tickets → use `/tickets`
- Transition ticket state → use `/ticket-transition`
- Implement, assign, prioritize in bulk, or manage a backlog
- Sync with external issue trackers
- Create branches, PRs, or commits from tickets
