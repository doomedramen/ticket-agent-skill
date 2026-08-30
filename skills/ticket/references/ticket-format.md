# Fallback ticket format

Use this format only when the current project has no documented ticket workflow
or template. A repository-local workflow always takes precedence.

## Directory layout

```text
./tickets/
├── open/
├── in-progress/
└── closed/
```

Create these directories only when `tickets/` does not already follow a local
convention.

## Filename and frontmatter

```text
{id}-{slug}.md
```

```yaml
---
id: "001"
title: "Fix login bug"
status: open
priority: medium
created: "2026-08-28T10:00:00Z"
updated: "2026-08-28T10:00:00Z"
tags: []
---
```

`id` is a zero-padded global counter found by scanning the three fallback
directories. `priority` defaults to `medium` unless the user states otherwise.

## Body

Use a `## Problem` section containing the user's request. If that request
depends on the current conversation, add a clearly labelled `## Context` or
`## Discussion proposal` section with the material needed to understand it.
Copy text verbatim when asked; otherwise retain only the discussion needed to
make the record useful. Do not invent acceptance criteria or implementation
plans.
