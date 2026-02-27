---
name: create-skill
description: Scaffold a new OpenCode skill in .agents/skills with valid naming, frontmatter, and reusable content.
compatibility: opencode
---

# Create Skill

## What I do
- Create a new skill folder under `.agents/skills/<name>/`.
- Generate a valid `SKILL.md` with required YAML frontmatter.
- Enforce OpenCode skill rules for naming and description length.
- Provide a practical body template so the new skill is immediately usable.

## Inputs
- `name` (required): skill name and directory name, for example `git-release`.
- `description` (required): short summary used by the `skill` tool discovery list.
- `license` (optional): frontmatter field.
- `compatibility` (optional): frontmatter field, for example `opencode`.
- `metadata` (optional): string-to-string map in frontmatter.
- `base_path` (optional): defaults to `.agents/skills`.

## Validation Rules
1. `name` must match: `^[a-z0-9]+(-[a-z0-9]+)*$`.
2. `name` length must be 1-64 characters.
3. Directory name must exactly match `name`.
4. `description` length must be 1-1024 characters.
5. `SKILL.md` must be uppercase and include frontmatter with at least:
   - `name`
   - `description`
6. Only supported frontmatter keys should be used:
   - `name`, `description`, `license`, `compatibility`, `metadata` (string-to-string map)

## Steps
1. Validate `name` and `description` against the rules.
2. Check `base_path/<name>/SKILL.md` does not already exist.
3. Create directory `base_path/<name>/`.
4. Write `SKILL.md` with valid frontmatter and clear usage instructions.
5. Confirm the new skill appears in `<available_skills>` (after reload/new session if needed).

## SKILL.md Template
```markdown
---
name: <skill-name>
description: <one-line description>
license: <optional>
compatibility: <optional>
metadata:
  audience: <optional>
  workflow: <optional>
---

# <Title>

## What I do
- <capability 1>
- <capability 2>

## When to use me
- <trigger 1>
- <trigger 2>

## Inputs
- `<input>`: <description>

## Steps
1. <step 1>
2. <step 2>

## Output
- <expected outcome>
```

## Example Invocation
```text
skill({ name: "<skill-name>" })
```

## Troubleshooting
- Skill not discovered: verify path is `.agents/skills/<name>/SKILL.md` and filename is uppercase.
- Skill hidden: check permission settings for `skill` in `opencode.json`.
- Load conflict: ensure skill names are unique across project and global skill locations.
