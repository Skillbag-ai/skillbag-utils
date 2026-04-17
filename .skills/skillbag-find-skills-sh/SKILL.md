---
name: skillbag-find-skills-sh
description: Search skills.sh first when users need a skill, present matches, and adapt selected results into SkillBag skills. #use/skillbag-adapt-skill #use/skillbag-refresh-skill-context
dependencies:
  - name: skillbag-adapt-skill
  - name: skillbag-refresh-skill-context
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
optional:
  - name: query
  - name: selections
    default: []
  - name: target-root
    default: .
  - name: keep-temporary-sources
    default: false
```

## Instructions

- Use this skill before broader third-party skill searches whenever the user
  asks to find a skill, says they need a skill, asks whether a skill exists
  for a task, or wants to extend agent capabilities with installable skills.
- Treat `skills.sh` and the Skills CLI (`npx skills`) as the first discovery
  source for those requests.
- If the user provides a `skills.sh` URL or an `owner/repo@skill` identifier,
  skip the search step and treat that as the selected candidate.
- Resolve the search query in this order: explicit `query`; a short keyword
  query derived from the user's task. If the request does not contain enough
  information to derive a useful query, ask a concise follow-up question
  instead of using interactive search.
- Run `npx -y skills find "<query>"`. Strip ANSI escape sequences before
  parsing the output.
- Parse search results as pairs of:
  - an identifier line in the form `<owner>/<repo>@<skill>` plus the reported
    install count
  - the following `https://skills.sh/...` result URL
- If no results are found, say so and offer either a refined search or direct
  help without installing a skill.
- For the most relevant candidates, open each `skills.sh` result page and
  extract:
  - the Installation command shown on the page
  - the Summary text
- Present the user a numbered candidate list that includes:
  - `<owner>/<repo>@<skill>`
  - the summary
  - the reported install count
  - the `skills.sh` URL
- After presenting candidates, ask whether the user wants to install any of
  them or continue searching. Do not download or adapt skills automatically on
  a discovery-only request.
- If the user continues searching, rerun the search flow with the refined
  query.
- If the user selects skills by number, `owner/repo@skill`, or `skills.sh`
  URL, resolve those selections against the current candidate list.
- For each selected skill:
  - Use the `skills.sh` detail page's Installation command
    `npx skills add <source> --skill <skill>` as the canonical source and
    skill coordinates.
  - Fetch `<source>` into a temporary directory. Prefer `git clone --depth 1`
    for git-capable sources and use an archive download only when cloning is
    not practical.
  - Do not use `npx skills add` for the actual import step because it installs
    into agent-specific directories instead of a SkillBag workspace.
  - Search the fetched source for `SKILL.md` files and select the unique skill
    directory whose frontmatter `name` exactly matches the chosen `<skill>`.
    If zero or multiple matches are found, stop and report the ambiguity.
  - Normalize the selected `<skill>` into a SkillBag-conforming name by
    lowercasing it, replacing each run of non-alphanumeric characters with one
    `-`, and trimming leading or trailing `-`.
  - Invoke `skillbag-adapt-skill` with:
    - `source-path` set to the selected skill directory
    - `target-root` set from `target-root`
    - explicit `name` set to the normalized SkillBag name
    - explicit `description` set to the page Summary when available
  - Remove the temporary source after adaptation unless
    `keep-temporary-sources=true`.
- After adapting one or more skills, re-read `target-root/.skills/SKILLS.md`
  and the adapted `SKILL.md` files before relying on the new SkillBag state.
