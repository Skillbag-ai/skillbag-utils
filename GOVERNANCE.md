# SkillBag Utils Governance

SkillBag Utils is a companion repository for reusable operational skills. It
is not the normative standard. The normative specification lives in the core
SkillBag repository.

## Scope

This repository may define useful workflows such as creating, adapting,
maintaining, discovering, refreshing, and supervising skills.

It must not silently redefine core SkillBag semantics. If a utility skill needs
behavior that changes the standard, propose that change in the core
`skillbag` repository first.

## Relationship to the Standard

Utility skills should:

- follow the current `SKILLBAG.md` rules
- keep `.skills/SKILLS.md` synchronized
- avoid hidden normative behavior
- document parameters and failure behavior clearly
- preserve compatibility with the base skill format where practical

## Maintainer Decisions

Maintainers may merge utility changes when they are focused, documented, and
compatible with the core standard.

Changes should be rejected or moved to the core standard when they:

- alter precedence rules
- redefine installation semantics
- change valid source requirements
- introduce conflicting runtime tag behavior
- make broad policy decisions that affect all SkillBag workspaces

## Releases

Release notes should identify:

- new utility skills
- breaking parameter or behavior changes
- compatibility updates required by the core standard
- security-relevant changes

## Sponsorship

Sponsorship can fund utility maintenance, tests, and documentation. It does not
grant private control over utilities or the core standard.

See [SUSTAINABILITY.md](./SUSTAINABILITY.md) for funding principles.

