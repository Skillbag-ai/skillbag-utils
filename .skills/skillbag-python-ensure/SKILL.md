---
name: skillbag-python-ensure
description: Ensure Python 3 is available for Python-dependent SkillBag workflows, asking before installation or guiding setup across Windows, macOS, and major Linux distros.
metadata:
  author: backupdev
  version: 1.0.0
---

## Parameters

```yaml
optional:
  - name: minimum-version
    default: "3.0"
  - name: target-root
    default: .
  - name: install-policy
    default: ask
    values:
      - ask
      - guide-only
```

## Instructions

- Use this skill before running Python scripts for another SkillBag skill, or
  when another skill declares `#use/skillbag-python-ensure`.
- Treat `minimum-version` as the minimum acceptable Python version. It MUST be
  Python 3 or newer.
- First detect the operating system and available shell. Then check for Python
  without changing the system:
  - Windows: try `py -3 --version`, `python --version`, then
    `python3 --version`.
  - macOS/Linux: try `python3 --version`, then `python --version`.
- Accept only executables that report Python 3.x and satisfy
  `minimum-version`. If `python` exists but reports Python 2.x, do not use it.
- When Python 3 is found, identify the preferred invocation and executable
  path when possible:
  - Windows: prefer `py -3`; use `py -3 -c "import sys; print(sys.executable)"`
    for the resolved executable path.
  - macOS/Linux: prefer `python3`; use
    `python3 -c "import sys; print(sys.executable)"` for the resolved path.
- Report the selected command and version to the user, then continue with the
  dependent workflow.

## Missing Python

- If Python 3 is missing or too old, do not run Python-dependent scripts.
- If `install-policy=guide-only`, give concise platform-specific installation
  steps and stop.
- Otherwise ask the user for permission before running any installer, package
  manager command, command that requires administrator privileges, or command
  that downloads software.
- Prefer these installation routes when the user approves:
  - Windows: use `winget search Python.Python`, choose the newest stable
    Python 3 package listed by winget, then install it with `winget install`.
    If winget is unavailable, guide the user to the official Python.org
    Windows installer and remind them to enable PATH integration.
  - macOS: if Homebrew is installed, use `brew install python`. Otherwise guide
    the user to the official Python.org macOS installer.
  - Ubuntu/Debian: use `sudo apt update` and `sudo apt install python3`.
  - Fedora/RHEL: use `sudo dnf install python3`.
  - Arch Linux: use `sudo pacman -S python`.
  - Alpine Linux: use `sudo apk add python3`.
  - openSUSE: use `sudo zypper install python3`.
- After an installation attempt, repeat the detection step in a fresh shell
  context when possible. If Python is still unavailable, explain the likely
  PATH, permission, or package-manager issue and give the next manual step.

## Dependency Guidance

- Skills that require Python SHOULD include `#use/skillbag-python-ensure` in
  their description and invoke this skill before any bundled Python script.
- If a dependent skill needs a newer Python version, pass an explicit
  `minimum-version` instead of weakening this skill's default.
- This skill ensures the interpreter only. It does not install project
  dependencies, create virtual environments, or manage Python packages unless
  another skill explicitly handles that work.
