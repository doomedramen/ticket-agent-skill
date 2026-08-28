# Ticket Format Reference

## Directory Layout

```
./tickets/
├── open/          # New tickets land here
├── in-progress/   # Actively being worked on
└── closed/        # Done, won't fix, or duplicate
```

Auto-created on first `/ticket` call if missing.

## File Naming

```
{id}-{slug}.md
```

- **id**: zero-padded 3-digit global counter (`001`, `002`, ... `100`)
- **slug**: title lowercased, spaces→hyphens, special chars stripped, max 60 chars
- Examples:
  - `001-fix-login-bug.md`
  - `002-add-dark-mode-toggle.md`
  - `003-database-migration-v2.md`

## Frontmatter

```yaml
---
id: "001"                          # string, zero-padded 3 digits
title: "Fix login bug"             # original title, not slugified
status: open                       # open | in-progress | closed
priority: medium                   # low | medium | high
created: "2026-08-28T10:00:00Z"   # ISO 8601
updated: "2026-08-28T10:00:00Z"   # ISO 8601, updated on every transition
tags: []                           # optional string array
---
```

### Fields

| Field | Required | Type | Notes |
|-------|----------|------|-------|
| `id` | yes | string | Zero-padded, globally unique across all dirs |
| `title` | yes | string | Original human-readable title |
| `status` | yes | enum | `open`, `in-progress`, `closed` |
| `priority` | yes | enum | `low`, `medium`, `high` |
| `created` | yes | ISO 8601 | Set once at creation |
| `updated` | yes | ISO 8601 | Updated on every transition |
| `tags` | no | string[] | Freeform, for filtering |

## Statuses

| Status | Meaning | Directory |
|--------|---------|-----------|
| `open` | Newly created, not started | `tickets/open/` |
| `in-progress` | Actively being worked on | `tickets/in-progress/` |
| `closed` | Done, won't fix, or duplicate | `tickets/closed/` |

## ID Generation

1. Scan `tickets/open/`, `tickets/in-progress/`, `tickets/closed/` for all `{id}-*.md` files
2. Extract numeric ID from each filename
3. Find maximum, increment by 1
4. Zero-pad to 3 digits
5. If no tickets exist, start at `001`

## Body Structure

```markdown
## Problem

Description of the issue, idea, or task.

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
```

Body is freeform markdown. The `## Problem` and `## Acceptance Criteria`
sections are conventional, not enforced. Agents may add additional sections
like `## Notes`, `## Context`, or `## References` as needed.
