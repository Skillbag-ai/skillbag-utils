# Contributing to SkillBag Utils

Thanks for contributing.

This repository contains reusable utility skills for SkillBag workspaces. Good
contributions keep the skills small, explicit, and easy to compose.

## Before You Start

- Read [README.md](./README.md).
- Read [AGENTS.md](./AGENTS.md).
- Review the current utility skills in [`.skills/`](./.skills/).

## What Good Contributions Look Like

Strong contributions usually do at least one of the following:

- add a reusable maintenance workflow that belongs outside the core spec
- tighten an existing utility skill without expanding its scope unnecessarily
- improve parameter clarity or failure behavior
- reduce duplicated instructions across skills
- keep `SKILLS.md` and skill metadata synchronized

## Skill Editing Rules

When editing or adding a utility skill:

- keep the canonical skill name stable unless a rename is intentional
- keep the `description` concise because it is part of the discovery surface
- preserve valid YAML frontmatter followed by Markdown instructions
- keep `metadata.version` in semantic version format
- update [`.skills/SKILLS.md`](./.skills/SKILLS.md) so it stays exact and sorted
- move large secondary detail into `references/`, `scripts/`, or `assets/`
  only when needed

## What To Avoid

Avoid changes that:

- duplicate rules that belong in the core SkillBag standard
- add broad or ambiguous skills with overlapping responsibilities
- silently change `#run/always` behavior
- leave skill descriptions or catalog entries out of sync

## Pull Requests

Pull requests should:

- stay focused on one utility or one logical behavior change
- update documentation affected by the change
- call out any parameter, metadata, or behavior change clearly

## Changelog

If the change is meaningful for users of this repository, add a short entry to
[CHANGELOG.md](./CHANGELOG.md).
