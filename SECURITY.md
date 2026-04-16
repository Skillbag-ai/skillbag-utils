# Security Policy

SkillBag Utils is a repository of operational skills, not a hosted service.
Security issues can still exist when a utility skill would cause an agent to:

- overwrite files unexpectedly
- make unsafe edits to repository state
- hide destructive behavior behind vague instructions
- run always-on behavior without clear disclosure
- mis-handle versioning or catalog synchronization in ways that break trust

## Reporting

If you find a security issue in a utility skill or repository instruction,
please report it privately to the maintainers before opening a public issue.

A useful report should include:

- the affected file or skill
- the security impact
- a concrete abuse or failure scenario
- any suggested mitigation

## Scope

Examples of in-scope issues:

- a skill that could overwrite unrelated files without clear instruction
- an always-run behavior that is not clearly disclosed
- guidance that could cause unsafe modification of a workspace
- misleading install or update behavior for utility skills

Examples of out-of-scope issues:

- generic concerns without a concrete repository impact
- third-party tool vulnerabilities unrelated to this repository's files

## Disclosure

Please allow maintainers time to evaluate and address the report before public
disclosure.
