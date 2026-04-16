---
name: refresh-skill-context
description: Re-ingest SkillBag state when SKILLBAG.md or local skill definitions change so the agent uses the latest installed rules and skills. #run/always
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
optional:
  - name: changed-paths
  - name: loaded-skills
```

## Instructions

- Use this skill to keep the agent's effective SkillBag context aligned with
  the current filesystem state while keeping context small.
- Treat these as re-ingest triggers:
  - `SKILLBAG.md`
  - `.skills/SKILLS.md`
  - any `.skills/<name>/SKILL.md`
  - skill installation, removal, rename, or overwrite that changes local
    `.skills/` contents
- If `changed-paths` is provided, use it as the explicit set of candidate
  changes to evaluate.
- Re-read `.skills/SKILLS.md` when the skill inventory may have changed.
- Re-read `SKILLBAG.md` if it changed or if skill execution rules may have been
  affected.
- Do not re-read every changed `SKILL.md` by default.
- Re-read a changed `SKILL.md` only if that skill is already loaded in the
  current working context or if it becomes newly selected for use after
  consulting `.skills/SKILLS.md`.
- If `loaded-skills` is provided, treat it as the explicit set of already
  loaded skills when deciding which changed `SKILL.md` files must be re-read.
- If a skill was removed, stop treating it as available in the current
  workspace context.
- Complete this re-ingest before relying on changed skill inventory or changed
  `SKILLBAG.md` semantics for subsequent work.
- If none of the trigger files changed, do nothing.
