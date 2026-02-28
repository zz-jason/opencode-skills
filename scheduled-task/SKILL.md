---
name: scheduled-task
description: Schedule recurring or delayed tasks by launching background opencode run processes via cron/systemd/at.
license: MIT
compatibility: opencode
metadata:
  audience: opencode-agents
  workflow: background-scheduling
---

# Scheduled Task

## What I do
- Convert a user request like "every N minutes" or "run after X" into OS-native scheduling.
- Use `cron`, `systemd --user` timers, or `at` to launch a new background `opencode run` process.
- Keep scheduler setup in the current opencode process (opencode 1), and run work in a separate process (opencode 2).
- Support task lifecycle: create, list, inspect, and cancel scheduled background tasks.

## When to use me
- User asks for periodic execution (hourly/daily/weekly/custom cron cadence).
- User asks for delayed one-shot execution (after 10 minutes / at a specific time).

## Required behavior
1. Verify actual CLI support first with:
   - `opencode --help`
   - `opencode run --help`
2. Never invent opencode subcommands. Use `opencode run ...` for the worker invocation.
3. Use shell-safe quoting and write a small runner script, then schedule that script.
4. Store artifacts under `~/.local/share/opencode-scheduled/<task-name>/`:
   - `run.sh` (runner)
   - `prompt.txt` (task prompt)
   - `logs/` (stdout/stderr)
5. Use a stable task identity across scheduler backends:
   - task name format: `[a-z0-9-]+`
   - scheduler unit/entry prefix: `opencode-scheduled-<task-name>`
   - cron entries must include marker comment: `# opencode-scheduled:<task-name>`

## Inputs
- `task_name`: stable id for scheduling artifacts.
- `task_prompt`: the message opencode 2 should execute.
- `run_dir`: working directory for opencode 2 (`--dir`).
- `schedule_type`: `recurring` or `once`.
- For recurring: `cron_expr` or human cadence converted to cron/systemd OnCalendar.
- For once: `delay` (e.g. `30 minutes`) or absolute time.
- Optional: `agent`, `model`, `variant`.

## Preferred scheduling strategy
- Linux with user systemd available: use `systemd --user` timer/service.
- Otherwise recurring: use `crontab` entry.
- One-shot delayed: prefer `at`; fallback `systemd-run --user --on-active`.

## Registry and discoverability
- Keep lightweight registry at `~/.local/share/opencode-scheduled/registry.tsv`.
- One line per task (tab-separated):
  - `task_name`
  - `backend` (`systemd_timer` | `cron` | `at`)
  - `schedule`
  - `created_at`
  - `handle` (unit name, cron marker, or at job id)
  - `run_dir`
- Update registry on create/cancel so users can always ask "what tasks are scheduled?".

## Runner template
```bash
#!/usr/bin/env bash
set -euo pipefail

TASK_DIR="$HOME/.local/share/opencode-scheduled/<task-name>"
LOG_DIR="$TASK_DIR/logs"
mkdir -p "$LOG_DIR"

ts="$(date +%Y%m%d-%H%M%S)"
nohup opencode run \
  --dir "<run-dir>" \
  --agent "<agent>" \
  --model "<model>" \
  -- "$(cat "$TASK_DIR/prompt.txt")" \
  >"$LOG_DIR/$ts.out" 2>"$LOG_DIR/$ts.err" < /dev/null &
```

## Cron example (recurring)
```bash
# Every hour at minute 5
5 * * * * /bin/bash "$HOME/.local/share/opencode-scheduled/<task-name>/run.sh"
```

## systemd user timer example (recurring)
```ini
# ~/.config/systemd/user/opencode-<task-name>.service
[Unit]
Description=Run scheduled opencode task <task-name>

[Service]
Type=oneshot
ExecStart=/bin/bash %h/.local/share/opencode-scheduled/<task-name>/run.sh
```

```ini
# ~/.config/systemd/user/opencode-<task-name>.timer
[Unit]
Description=Schedule opencode task <task-name>

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

## One-shot examples
```bash
# after 30 minutes (at)
echo "/bin/bash $HOME/.local/share/opencode-scheduled/<task-name>/run.sh" | at now + 30 minutes

# after 30 minutes (systemd-run fallback)
systemd-run --user --on-active=30m /bin/bash "$HOME/.local/share/opencode-scheduled/<task-name>/run.sh"
```

## List scheduled tasks
When user asks for scheduled background tasks, query both registry and live scheduler state:

```bash
# 1) Registry snapshot (if exists)
cat "$HOME/.local/share/opencode-scheduled/registry.tsv"

# 2) systemd user timers created by this skill
systemctl --user list-timers --all 'opencode-scheduled-*'

# 3) cron entries created by this skill
crontab -l | grep 'opencode-scheduled:'

# 4) at queue (if used)
atq
```

Output should include: task name, backend, cadence/when, next run, and cancellation command.

## Cancel scheduled tasks
When user asks to cancel a task, resolve by backend then remove scheduling object and registry row.

### Cancel systemd timer task
```bash
systemctl --user disable --now opencode-scheduled-<task-name>.timer
rm -f "$HOME/.config/systemd/user/opencode-scheduled-<task-name>.timer"
rm -f "$HOME/.config/systemd/user/opencode-scheduled-<task-name>.service"
systemctl --user daemon-reload
```

### Cancel cron task
```bash
crontab -l | sed '/opencode-scheduled:<task-name>/d' | crontab -
```

### Cancel one-shot at task
```bash
atrm <job-id>
```

After cancel, delete the task line from `registry.tsv` and confirm removal with a fresh list.

## Output to user
- What was scheduled, with exact scheduler entry/unit name.
- Where logs and prompt are stored.
- How to list, inspect, disable, and remove the schedule.
- For list requests: a concise table of currently scheduled tasks.
- For cancel requests: explicit confirmation of which task/backend/handle was removed.

## Safety notes
- Do not overwrite existing schedule silently; update or create with clear naming.
- Keep prompts in files instead of shell inline strings for robust quoting.
- If using cron and overlap is risky, wrap `run.sh` with `flock`.
- For cancel requests, match exact task name; do not wildcard-delete unrelated scheduler entries.
