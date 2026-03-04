---
name: skill-sync
description: Sync all local skills from the canonical GitHub repository into ~/.agents/skills.
---

# Skill Sync

## What I do
- Update local skill files from the canonical repository: `https://github.com/zz-jason/opencode-skills`.
- Sync the installed skills directory (default: `~/.agents/skills`) to the latest remote state.
- Preserve or clean local changes based on user preference.

## When to use me
- You want the latest shared skills from the upstream repository.
- Your local skills are outdated compared with GitHub.
- You need a repeatable “update all skills” workflow.

## Inputs
- `target_dir` (optional): local skills directory. Default: `~/.agents/skills`.
- `repo_url` (optional): source repo URL. Default: `https://github.com/zz-jason/opencode-skills.git`.
- `branch` (optional): branch to sync from. Default: `main`.
- `mode` (optional):
  - `safe` (default): stash local changes, rebase on remote, then re-apply stash.
  - `clean`: discard local changes and hard reset to `origin/<branch>`.

## Steps
1. Resolve `target_dir` and verify it is a git repository.
2. Ensure `origin` points to `repo_url`; if not, either update origin or stop and ask for confirmation.
3. Run `git fetch origin --prune`.
4. Switch to the target branch (create it from `origin/<branch>` if missing).
5. Apply sync mode:
   - `safe`:
     1. `git stash push -u -m "skill-sync-auto-stash"` (only if dirty)
     2. `git rebase origin/<branch>`
     3. `git stash pop` (if stash created)
   - `clean`:
     1. `git reset --hard origin/<branch>`
     2. `git clean -fd`
6. Report the result with old/new commit IDs and whether local changes were restored or discarded.

## Output
- Local skills synced to the selected upstream branch.
- A short sync report including:
  - repo URL
  - branch
  - previous commit -> new commit
  - mode used
  - local change handling result

## Example Invocation
```text
skill({
  name: "skill-sync",
  target_dir: "~/.agents/skills",
  branch: "main",
  mode: "safe"
})
```

## Troubleshooting
- Not a git repo: clone first into `~/.agents/skills`.
- Rebase conflicts: resolve conflicts and rerun sync.
- Stash pop conflicts: keep conflict markers, resolve manually, and continue.
