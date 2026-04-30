---
name: skillbag-modify-skill
description: Modify an existing skill in a SkillBag repo while preserving compliance and catalog sync.
metadata:
  author: backupdev
  version: 1.0.0
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
- If a change belongs in a broader existing skill or dependency, ask whether to
  factor it there, depend on it, or keep the edit local.
- Ask before broadening scope, changing dependencies, renaming, or editing
  additional skills.
- Do not change unrelated skills.
- Rewrite `target-root/.skills/SKILLS.md` so it remains sorted and exactly matches the local `.skills/` contents.
