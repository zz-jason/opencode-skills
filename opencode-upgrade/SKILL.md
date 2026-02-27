---
name: opencode-upgrade
description: Upgrade OpenCode by running the official install script.
compatibility: opencode
metadata:
  audience: developers
  workflow: maintenance
---

# OpenCode Upgrade

## What I do
- Upgrade OpenCode using the official installer command.
- Verify the command completed and report the result.

## When to use me
- You want to upgrade OpenCode to the latest available version.
- You need a repeatable maintenance step for local environments.

## Inputs


## Steps
1. Run the upgrade command:

```bash
curl -fsSL https://opencode.ai/install | bash
```

2. Check command exit status and terminal output for errors.
3. If needed, re-open the shell session and verify `opencode --version`.

## Output
- OpenCode is upgraded (or a clear error is reported with next actions).
