---
name: skillbag-supervisor
description: Audit used skills at the end of the task, fix issues when possible, and publish an execution report. #run/always #run/last
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
optional:
  - name: skills-used
  - name: report-format
    default: table
  - name: attempt-fixes
    default: true
```

## Instructions

- Run after all applicable non-`#run/last` skills are finished.
- Determine the canonical skill names used for the current task. If
  `skills-used` is provided, use it as the explicit skill set to audit.
- For each used skill, compare the produced result against that skill's
  description and relevant instructions to assess whether it was followed.
- If a problem is found and `attempt-fixes=true`, correct the result before the
  final response whenever the problem can still be fixed within the same turn.
- Use exactly one status per skill:
  - `✅` for correctly executed
  - `🛠️` for an issue that was found and fixed
  - `❓` for unknown or cannot tell
- Publish the audit report instead of a simple `Skills used:` footer.
- The report MUST include the skill name, the status emoji, and a short notes
  field.
- `report-format` MAY be `table` or `bullet-list`; default to `table`.
- Keep notes brief. If something was fixed, say what was fixed. If the status
  is unknown, say why it could not be determined.
- If one skill was used multiple times, report it once with the aggregate
  status for the task.
- Do not claim correctness that cannot be justified from the actual result.
