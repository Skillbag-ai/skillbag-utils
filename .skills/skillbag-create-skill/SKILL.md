---
name: skillbag-create-skill
description: Create a new skill in a SkillBag repo and keep SKILLS.md synchronized.
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
required:
  - name: name
  - name: description
optional:
  - name: target-root
    default: .
  - name: version
    default: 1.0.0
  - name: intended-contexts
  - name: configuration
  - name: dependencies
  - name: instructions
```

## Instructions

- Treat `target-root` as the SkillBag workspace root.
- Ensure `target-root/.skills/` and `target-root/.skills/SKILLS.md` exist.
- Before creating the skill, read `target-root/.skills/SKILLS.md` and inspect
  likely similar `target-root/.skills/*/SKILL.md` files by name and
  description.
- Prefer the most reusable shape clearly justified by the request; do not
  overgeneralize from one narrow use case.
- If similar behavior exists, ask whether to modify that skill, create a
  wrapper/dependent skill, or continue with a new standalone skill.
- Create the skill at `target-root/.skills/<name>/SKILL.md`.
- `name` MUST match `[a-z0-9]+(?:-[a-z0-9]+)*`, be 1-64 characters, and become the `name` field in `SKILL.md`.
- `description` MUST be non-empty and become both the `description` field in `SKILL.md` and the catalog text in `SKILLS.md`.
- `version` MUST default to `1.0.0` and be written to `metadata.version` in the new `SKILL.md`.
- `SKILL.md` MUST use YAML frontmatter followed by Markdown instructions.
- Put context-specific behavior in parameters, references, scripts, or assets
  instead of hard-coding it into the core workflow.
- Declare dependencies when reusing existing skills; use `#use/<skill-name>` in
  the description when companion execution should be considered.
- Keep the body short; move secondary detail into `references/`, `scripts/`, or `assets/` when needed.
- Create optional subdirectories only when they are needed.
- Update `target-root/.skills/SKILLS.md` so it contains exactly one sorted line `<name>: <description>` for the new skill.
- If scope, dependency choice, naming, or parametrization is unclear, ask the
  user with concise options before changing files.
