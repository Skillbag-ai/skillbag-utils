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
- Preserve the canonical skill `name` unless `new-name` is explicitly requested.
- If `new-name` is provided, rename the directory, update the `name` field, and replace the catalog entry atomically.
- Keep `description` non-empty. If `new-description` is provided, update both `SKILL.md` and `target-root/.skills/SKILLS.md`.
- `new-version` MAY set `metadata.version` directly.
- `version-bump` MAY be `patch`, `minor`, or `major`.
- If both `new-version` and `version-bump` are provided, stop and report the conflict.
- If `version-bump` is provided, the existing `metadata.version` MUST be valid semantic versioning; apply the bump and write the result back to `metadata.version`.
- Preserve YAML frontmatter plus Markdown body format.
- Keep the body short; move secondary detail into `references/`, `scripts/`, or `assets/` when needed.
- Do not change unrelated skills.
- If the edited skill still duplicates shared instructions, factor the shared text into a dependency or reference file instead of copying it verbatim.
- Rewrite `target-root/.skills/SKILLS.md` so it remains sorted and exactly matches the local `.skills/` contents.
