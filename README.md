# 🎒 SkillBag Utils

SkillBag Utils is a companion SkillBag repository that provides reusable
utility skills for maintaining SkillBag workspaces.

This repository is itself a valid SkillBag source:

- repository instructions live in [AGENTS.md](./AGENTS.md)
- installed skills live under [`.skills/`](./.skills/)
- the skill catalog lives at [`.skills/SKILLS.md`](./.skills/SKILLS.md)

The utilities here are meant to be installed into other workspaces as
dependencies. They focus on common maintenance workflows rather than core
standard definition.

## Available Skills

### [skillbag-adapt-skill](./.skills/skillbag-adapt-skill/SKILL.md)

Adapts an existing file or directory into a conforming SkillBag skill
directory while preserving source material and synchronizing the local
`SKILLS.md` catalog.

Key parameters:

- `source-path` is required
- `name` and `description` can override or fill in missing metadata
- `move-source` defaults to `false`

Behavior:

- reuses a root source `SKILL.md` when present and normalizes its frontmatter
- otherwise generates a short wrapper `SKILL.md`
- classifies remaining source files into `scripts/`, `references/`, and
  `assets/` deterministically
- stops on missing description, target collisions, or destination conflicts
  instead of guessing

Use this when you have a loose file or directory that should become a proper
SkillBag skill without manually reorganizing it first.

### [skillbag-create-skill](./.skills/skillbag-create-skill/SKILL.md)

Creates a new local skill directory, writes a conforming `SKILL.md`, and keeps
the local `SKILLS.md` catalog synchronized.

Key parameters:

- `name` and `description` are required
- `target-root` selects the SkillBag workspace to modify
- `version` defaults to `1.0.0`
- `instructions` lets you seed the skill body

Use this when a workspace needs a new skill scaffold with the correct name
format, frontmatter, and catalog update in one step.

### [skillbag-find-skills-sh](./.skills/skillbag-find-skills-sh/SKILL.md)

Searches [skills.sh](https://skills.sh/) first when a user is looking for an
installable skill, then adapts selected results into conforming SkillBag
skills instead of installing them into agent-specific directories.

Key parameters:

- `query` can override the search terms
- `selections` can identify which found skills to adapt
- `target-root` defaults to `.`

Behavior:

- uses `npx skills find` as the first external skill-discovery step for
  "find me a skill" requests
- presents a candidate list and asks the user whether to install any of them
  or continue searching
- resolves selected results through the `skills.sh` detail page's published
  installation command
- downloads the selected source and adapts the chosen skill with
  `skillbag-adapt-skill`

Use this when users want to discover reusable skills from the public
`skills.sh` ecosystem but the destination needs to stay in SkillBag format.

### [skillbag-log-skills](./.skills/skillbag-log-skills/SKILL.md)

Adds a simple skills-used footer to the final response. This is the lightweight
reporting option when you only want a flat list of which skills were involved.

Behavior:

- tagged `#run/always`
- de-duplicates skill names
- uses canonical hyphenated names
- can be disabled through user context

Use this when you want minimal visibility into which skills ran, without the
heavier correctness audit performed by `skillbag-supervisor`.

### [skillbag-long-task](./.skills/skillbag-long-task/SKILL.md)

Breaks clearly oversized work into continuation-sized chunks so the agent can
make steady progress without overrunning response limits.

Key parameters:

- `continuation-messages` defaults to `next`, `continue`, `>`, and `.`
- `chunk-boundary-hint` can bias chunking toward files, pages, rows, or any
  other natural unit

Use this for batch-style work such as transcribing hundreds of audio files,
processing very large document sets, or any task where one reply is not enough.
A skill that benefits from this can advertise that relationship with a
`#use/skillbag-long-task` tag.

### [skillbag-modify-skill](./.skills/skillbag-modify-skill/SKILL.md)

Edits an existing skill while preserving format compliance and catalog
consistency. It handles both small textual updates and structured changes such
as renaming or version bumps.

Key parameters:

- `name` is required
- `new-name` and `new-description` update identity and catalog text
- `new-version` sets a version directly
- `version-bump` supports `patch`, `minor`, or `major`
- `instructions` updates the skill body

Use this when a skill already exists and you want controlled changes without
manually reconciling `SKILL.md` and `SKILLS.md`.

### [skillbag-refresh-skill-context](./.skills/skillbag-refresh-skill-context/SKILL.md)

Refreshes the agent's understanding of the current SkillBag workspace after
`SKILLBAG.md`, `SKILLS.md`, or installed local skills change, while keeping
context usage small.

Key parameters:

- `changed-paths` can identify the specific files that changed
- `loaded-skills` can identify which skills were already loaded in the current
  working context

This skill always re-checks cheap discovery state first, then only re-reads a
changed `SKILL.md` when that skill was already loaded or becomes newly selected
for use. Use it after skill installation, removal, rename, overwrite, or any
change to `SKILLBAG.md` execution semantics.

### [skillbag-supervisor](./.skills/skillbag-supervisor/SKILL.md)

Performs an end-of-task audit of the skills that were actually used and
publishes a correctness report instead of a plain skills-used footer.

Key parameters:

- `skills-used` can provide the explicit set of skills to audit
- `report-format` supports `table` or `bullet-list`
- `attempt-fixes` defaults to `true`

Behavior:

- tagged `#run/always #run/last`
- checks whether each used skill was followed closely enough
- fixes problems in the same turn when possible
- reports one status per skill: correct, fixed, or unknown

Use this when you want final-response quality control rather than simple skill
logging.

## How To Use

Typical usage is to add this repository as a SkillBag dependency from another
workspace, then install the needed skills into that workspace's `.skills/`
directory.

You can also use this repository directly as a local SkillBag source because
it follows the same repository structure expected by the SkillBag standard.

## Repository Layout

- [AGENTS.md](./AGENTS.md): repository-level installation metadata
- [README.md](./README.md): project overview
- [CONTRIBUTING.md](./CONTRIBUTING.md): contribution guidance
- [GOVERNANCE.md](./GOVERNANCE.md): utility-repository governance
- [SUSTAINABILITY.md](./SUSTAINABILITY.md): funding and maintenance model
- [CODE_OF_CONDUCT.md](./CODE_OF_CONDUCT.md): collaboration standards
- [SECURITY.md](./SECURITY.md): security reporting guidance
- [CHANGELOG.md](./CHANGELOG.md): notable repository changes
- [LICENSE.md](./LICENSE.md): MIT license
- [`.skills/SKILLS.md`](./.skills/SKILLS.md): low-cost skill discovery catalog

## Design Notes

These skills are intentionally small and task-specific. Shared repository rules
belong in the core SkillBag standard. Reusable operational workflows belong
here.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## Security

See [SECURITY.md](./SECURITY.md).

## License

Released under the MIT license. See [LICENSE.md](./LICENSE.md).
