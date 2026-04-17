# Changelog

All notable changes to this repository should be documented in this file.

The format is intentionally simple while the project remains a draft.

## v0.2.0

- Added `skillbag-adapt-skill` for adapting a file or directory into a
  conforming SkillBag skill while preserving source material and keeping
  `SKILLS.md` synchronized.
- Added `skillbag-find-skills-sh` for searching `skills.sh` first when users
  ask for an installable skill, then adapting selected results into SkillBag
  format instead of installing them into agent-specific directories.
- Renamed `refresh-skill-context` to
  `skillbag-refresh-skill-context`.
- Updated the repository version marker to `SKILLBAG UTILS v0.2.0` in
  `AGENTS.md`.

## v0.1.1

- Initial versioned release of `skillbag-utils` as a SkillBag source
  repository.
- Added the utility skills `skillbag-create-skill`, `skillbag-modify-skill`,
  and `skillbag-log-skills`.
- Added repository-level OSS documentation, including README, contributing,
  license, code of conduct, security guidance, and changelog.
- Added `skillbag-long-task` for chunking clearly long-running tasks into
  continuation-sized responses.
- Added `refresh-skill-context` for low-cost re-ingest of changed SkillBag
  state, with lazy `SKILL.md` re-reading for only already loaded or newly
  selected skills.
- Added `skillbag-supervisor` as a `#run/always #run/last` execution-audit
  skill that can replace a simple skills-used footer with a per-skill status
  report.
- Added the repository version marker `SKILLBAG UTILS v0.1.1` to `AGENTS.md`.
