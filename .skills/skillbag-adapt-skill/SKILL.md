---
name: skillbag-adapt-skill
description: Adapt a file or directory into a conforming SkillBag skill and keep SKILLS.md synchronized.
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
required:
  - name: source-path
optional:
  - name: target-root
    default: .
  - name: name
  - name: description
  - name: move-source
    default: false
```

## Instructions

- Treat `target-root` as the SkillBag workspace root and `source-path` as the
  path to adapt.
- `source-path` MUST exist and be either a regular file or directory;
  otherwise stop and report.
- Resolve the target skill name in this order: explicit `name`; `name` from a
  root source `SKILL.md`; normalized basename of `source-path`. For a file
  source, use the filename stem; for a directory source, use the directory
  basename. Normalize by lowercasing, replacing each run of non-alphanumeric
  characters with one `-`, and trimming leading or trailing `-`. If the result
  is empty, longer than 64 characters, or still fails
  `[a-z0-9]+(?:-[a-z0-9]+)*`, stop and ask the user for an explicit `name`.
  The resolved name becomes both the target directory name and the target
  `SKILL.md` `name`.
- Resolve the target description in this order: explicit `description`;
  `description` from a root source `SKILL.md`. If neither yields a non-empty
  description, stop and ask the user for one instead of inventing it.
- Ensure `target-root/.skills/` and `target-root/.skills/SKILLS.md` exist.
- Create the target at `target-root/.skills/<name>/`. If that path already
  exists, stop and report before changing files.
- A root source `SKILL.md` is authoritative for body content. Normalize its
  frontmatter to the resolved `name` and `description` while preserving its
  Markdown body. Only a root `SKILL.md` gets this treatment; nested
  `SKILL.md` files are preserved as regular source material.
- If no root source `SKILL.md` exists, generate a short conforming `SKILL.md`
  that points to preserved material in `scripts/`, `references/`, and
  `assets/` instead of copying long source text into the body.
- Preserve source material under the optional SkillBag subdirectories:
  - keep existing `scripts/`, `references/`, and `assets/` directory contents
    in the same bucket while preserving paths relative to that bucket root
  - classify remaining files as follows:
    - `*.md`, `*.mdx`, `*.txt`, `*.rst` -> `references/`
    - files with an executable bit, a shebang, or extensions `.sh`, `.bash`,
      `.zsh`, `.fish`, `.py`, `.js`, `.mjs`, `.cjs`, `.ts`, `.tsx`, `.rb`,
      `.pl`, `.php`, `.go`, `.rs`, `.java`, `.kt`, `.swift`, `.ps1`, `.lua`
      -> `scripts/`
    - all other files -> `assets/`
- When classifying files from a directory outside existing `scripts/`,
  `references/`, or `assets/` directories, preserve each file's path relative
  to the source root inside its destination bucket.
- Ignore only VCS metadata and common junk files: `.git/`, `.hg/`, `.svn/`,
  `.DS_Store`, `Thumbs.db`, files ending in `~`, `.swp`, or `.tmp`.
- Never silently drop non-ignored source material. If two source paths would
  map to the same destination path, stop and report the conflict.
- If `move-source=true`, move source material into the new skill directory;
  otherwise copy it.
- Update `target-root/.skills/SKILLS.md` so it remains sorted and exactly
  matches the local `.skills/` contents.
