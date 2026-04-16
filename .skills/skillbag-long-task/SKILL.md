---
name: skillbag-long-task
description: Split clearly long-running or context-heavy tasks into response-sized chunks and ask the user to continue.
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
optional:
  - name: continuation-messages
    default:
      - next
      - continue
      - ">"
      - "."
  - name: chunk-boundary-hint
```

## Instructions

- Use this skill when the task clearly spans many items or the useful response
  would be too large for one reply.
- Prefer natural chunk boundaries such as files, pages, rows, or items.
- If `chunk-boundary-hint` is provided, follow it when practical.
- Size each chunk to use most of the available response budget while leaving
  room for a short progress handoff.
- After each chunk, state what was completed, what remains, and that the user
  can continue with one of the values in `continuation-messages`.
- If the user sends a continuation message from `continuation-messages`,
  resume from the next unfinished chunk with minimal repeated context.
- For batch work, keep stable progress markers such as counts, filenames, or
  processed ranges.
- Do not use this skill for short tasks that can be completed cleanly in one
  response.
