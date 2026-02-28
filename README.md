# OpenCode Skills

This repository contains a collection of skills for [OpenCode](https://opencode.ai/), an interactive CLI tool for software engineering tasks.

## Installation

Run these two commands:

```bash
mkdir -p ~/.config/opencode/skills
git clone https://github.com/zz-jason/opencode-skills.git ~/.config/opencode/skills
```

## Available Skills

| Skill Name | Description |
|------------|-------------|
| `create-skill` | Scaffold a new OpenCode skill with valid naming, frontmatter, and reusable content. |
| `design` | Write design documents using the bundled design template. |
| `opencode-upgrade` | Upgrade OpenCode by running the official install script. |
| `agents-docs-init` | Initialize `AGENTS.md` and a minimal `.agents` knowledge base for agent collaboration. |
| `agents-docs-refresh` | Refresh `.agents` docs to remove stale, incorrect, and duplicate content. |
| `scheduled-task` | Schedule delayed or recurring background tasks by launching `opencode run` via cron/systemd/at, with list/cancel support. |
| `xiaohongshu-search` | Search Xiaohongshu notes by keyword, time, and intent, then return structured note summaries and links. |

## Usage

Once installed, OpenCode will automatically detect these skills. You can check installed skills with:

```
/skills
```

OpenCode automatically determines when to use skills based on keywords in the skill's description and context during task processing. Each skill's behavior and triggers are defined in its `SKILL.md` file.

## Contributing

You can create new skills using the `create-skill` skill in this repository, which follows the [OpenCode skill specification](https://opencode.ai/docs/skills/). Alternatively, manually create a directory with a `SKILL.md` file that includes the required frontmatter and sections.

## License

MIT
