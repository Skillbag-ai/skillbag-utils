---
name: skillbag-modify-skill
description: Modify an existing skill in a SkillBag repo while preserving compliance and catalog sync.
metadata:
  author: backupdev
  version: 1.1.1
---

## Parameters

```yaml
required:
  - name: name
optional:
  - name: target-root
    default: .
  - name: new-name
  - name: new-description
  - name: new-version
  - name: version-bump
  - name: instructions
```

## Instructions

- Treat `target-root` as the SkillBag workspace root.
- Load the existing skill from `target-root/.skills/<name>/SKILL.md`.
- Before editing, read `target-root/.skills/SKILLS.md` and inspect neighboring
  skills when the requested change introduces reusable behavior or overlaps
  another skill.
- Preserve the canonical skill `name` unless `new-name` is explicitly requested.
- If `new-name` is provided, rename the directory, update the `name` field, and replace the catalog entry atomically.
- Keep `description` non-empty. If `new-description` is provided, update both `SKILL.md` and `target-root/.skills/SKILLS.md`.
- `new-version` MAY set `metadata.version` directly.
- `version-bump` MAY be `patch`, `minor`, or `major`.
- If both `new-version` and `version-bump` are provided, stop and report the conflict.
- If `version-bump` is provided, the existing `metadata.version` MUST be valid semantic versioning; apply the bump and write the result back to `metadata.version`.
- Preserve YAML frontmatter plus Markdown body format.
- Keep the body short; move secondary detail into `references/`, `scripts/`, or `assets/` when needed.
- Prefer parameters, references, scripts, assets, and dependency declarations
  over duplicated shared instructions.
- When a requested change makes the skill context-heavy but the behavior can be
  described as deterministic steps, prefer a script-backed skill over
  prose-only instructions. Good candidates include repeated parsing,
  validation, file generation, catalog synchronization, batch transforms, and
  other mechanical work where code can reduce future agent context and token
  use.
- For script-backed skills, keep `SKILL.md` focused on when to run the helper,
  required inputs, expected outputs, and failure behavior. Put implementation
  detail in `scripts/`, with supporting notes in `references/` only when
  needed.
- Do not create scripts for ambiguous judgment work, open-ended writing,
  one-off narrow tasks, or workflows where the agent must inspect and decide
  case by case.
- If the modification adds or preserves Python scripts, Python commands, or a
  deterministic helper script that is best implemented in Python, ensure the
  skill description and `SKILLS.md` catalog entry include
  `#use/skillbag-python-ensure` unless already present.
- Treat `#use/skillbag-python-ensure` as an implied dependency when Python use
  is clear. Ask before adding it when Python is optional, ambiguous, or not
  clearly the more context-efficient implementation.
- If a change appears to remove all Python usage from a skill that already
  declares `#use/skillbag-python-ensure`, ask before removing that dependency.
- If a change belongs in a broader existing skill or dependency, ask whether to
  factor it there, depend on it, or keep the edit local.
- Ask before broadening scope, changing dependencies, renaming, or editing
  additional skills.
- Do not change unrelated skills.
- Rewrite `target-root/.skills/SKILLS.md` so it remains sorted and exactly matches the local `.skills/` contents.
