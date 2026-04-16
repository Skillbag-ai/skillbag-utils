# SkillBag Utils

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

- [skillbag-create-skill](./.skills/skillbag-create-skill/SKILL.md): Create a new skill in a SkillBag repo and keep `SKILLS.md` synchronized.
- [skillbag-log-skills](./.skills/skillbag-log-skills/SKILL.md): Make the agent always output the list of skill names used to generate the response. `#run/always`
- [skillbag-long-task](./.skills/skillbag-long-task/SKILL.md): Split clearly long-running or context-heavy tasks into response-sized chunks and ask the user to continue.
- [skillbag-modify-skill](./.skills/skillbag-modify-skill/SKILL.md): Modify an existing skill in a SkillBag repo while preserving compliance and catalog sync.
- [refresh-skill-context](./.skills/refresh-skill-context/SKILL.md): Re-ingest SkillBag state after `SKILLBAG.md`, `SKILLS.md`, or loaded local skills change, while keeping `SKILL.md` reads lazy. `#run/always`
- [skillbag-supervisor](./.skills/skillbag-supervisor/SKILL.md): Audit used skills at the end of the task, fix issues when possible, and publish an execution report. `#run/always` `#run/last`

Example: a transcription skill can include `#use/skillbag-long-task` so very
large jobs such as hundreds of audio files are processed in continuation-sized
chunks instead of one oversized response.

Example: a workspace can install `skillbag-supervisor` to replace a simple
skills-used footer with a final execution audit that marks each used skill as
correct, fixed, or unknown.

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
