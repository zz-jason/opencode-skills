---
name: agents-docs-init
description: Initialize AGENTS.md and a minimal .agents knowledge base for agent collaboration.
compatibility: opencode
metadata:
  audience: project-maintainers
  workflow: agent-bootstrap
---

# Initialize Agent Docs

## What I do
- Create a standard agent-collaboration scaffold at the project root: `AGENTS.md` and `.agents/`.
- Generate core knowledge files: `KNOWLEDGE_BASE.md` and `CODING_STYLE.md`.
- Embed default execution policy for coding tasks: commit at the end of each task round.
- Embed default execution policy for coding tasks: each round must write newly identified factual information into `.agents/KNOWLEDGE_BASE.md`.
- Embed hard gate for coding tasks: if KB updates are missing for the round, commit is blocked.
- Embed default execution policy for coding tasks: no backward compatibility work and no extra protective logic unless explicitly required.
- Keep the scaffold minimal and focused on shared knowledge files.
- Keep setup safe by default: do not overwrite existing files unless explicitly requested.

## When to use me
- You are introducing coding-agent workflows into a new repository.
- Your repository has no `AGENTS.md` yet, or has inconsistent agent conventions.
- You want a predictable context baseline before using other skills.

## Inputs
- `project_name` (optional): display name used in `AGENTS.md`. Default: repository folder name.
- `target_path` (optional): repository root path. Default: `.`.
- `overwrite` (optional): whether to replace existing files. Default: `false`.
- `tech_stack` (optional): stack/tooling notes to pre-fill in `AGENTS.md`.
- `anti_patterns` (optional): project-specific anti-patterns to pre-fill in `AGENTS.md`.

## Files created
- `AGENTS.md`
- `.agents/KNOWLEDGE_BASE.md`
- `.agents/CODING_STYLE.md`

## Steps
1. Resolve `target_path` and verify it is a project root.
2. Check whether each target file already exists.
3. If `overwrite=false`, skip existing files and only create missing ones.
4. Copy core knowledge templates into `.agents/`.
5. Fill `AGENTS.md` placeholders (`{{PROJECT_NAME}}`, stack, anti-patterns) with provided inputs.
6. Recommend periodic cleanup using the sibling skill `agents-docs-refresh`.

## Output
- A ready-to-use agent-collaboration baseline that follows the workflow: context -> design -> coding -> validation -> feedback.

## Example Invocation
```text
skill({ name: "agents-docs-init" })
```

## Troubleshooting
- Missing skill in discovery list: verify file path is `skills/agents-docs-init/SKILL.md`.
- Existing files were not changed: this is expected when `overwrite=false`.
- Low-quality outputs from agents: complete and maintain `.agents/KNOWLEDGE_BASE.md` and `.agents/CODING_STYLE.md`.
