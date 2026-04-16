---
name: skillbag-log-skills
description: Make the agent always output the list of skill names used to generate the response. #run/always
metadata:
  author: backupdev
  version: 1.0.0
---

## Instructions

- Track the canonical names of all skills used to produce the current response.
- Append a flat list of those skill names to every final response. Format: "Skills used: <skill-name-1>, <skill-name-2>, ..."
- Use canonical hyphenated skill names in that list.
- Do not list the same skill more than once.
- Skip the list only when the user context explicitly disables `#run/always` skills.
