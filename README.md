# Agent Skills

This repository contains a collection of reusable skills for coding agents.

## Installation

Install this repository into `~/.agents/skills`:

```bash
mkdir -p ~/.agents
git clone https://github.com/zz-jason/opencode-skills.git ~/.agents/skills
```

## Available Skills

| Skill Name | Description |
|------------|-------------|
| `skill-create` | Scaffold a new skill with valid naming, frontmatter, and reusable content. |
| `skill-update` | Improve an existing skill using real usage feedback, failures, and observed gaps. |
| `skill-sync` | Sync all local skills from the canonical GitHub repository into `~/.agents/skills`. |
| `design` | Write design documents using the bundled design template. |
| `opencode-upgrade` | Run the official install script to perform tool upgrades. |
| `agents-docs-init` | Initialize `AGENTS.md` and a minimal `.agents` knowledge base for agent collaboration. |
| `agents-docs-refresh` | Refresh `.agents` docs to remove stale, incorrect, and duplicate content. |
| `scheduled-task` | Schedule delayed or recurring background tasks via cron/systemd/at, with list/cancel support. |
| `code-review` | Perform critical Linus-style code review on selected git diff target and save a markdown report with file-line findings and action items. |
| `hierarchical-orchestration` | Plan and execute complex work with a coarse worker DAG and adaptive sub-workers under strict budget and handoff controls. |

## Usage

Once installed, your agent will automatically detect these skills. You can check installed skills with:

```
/skills
```

Agents can decide when to use a skill based on the skill description and current task context. Each skill's behavior and triggers are defined in its `SKILL.md` file.

## Contributing

You can create new skills using `skill-create`, iteratively improve existing ones using `skill-update`, and update your local skill set from upstream with `skill-sync`. Alternatively, manually create a directory with a `SKILL.md` file that includes the required frontmatter and sections.

## License

MIT
