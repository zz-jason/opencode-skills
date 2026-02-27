# AGENTS.md

Project: {{PROJECT_NAME}}

This file defines the standard workflow for collaborating with coding agents in this repository.

## Design Philosophy

- Process over Prompt: rely on repeatable workflows, not one-off prompt tricks.
- Context First: agent quality depends on context quality.
- Validation over Trust: verify with build/test/lint and review.
- Feedback-driven Iteration: continuously update shared knowledge from task outcomes.

## Working Directory Structure

```
.
├── AGENTS.md
└── .agents/
    ├── KNOWLEDGE_BASE.md
    └── CODING_STYLE.md
```

## Project Overview

- Product/domain: <fill this>
- Primary users: <fill this>
- Runtime environments: <fill this>

## Tech Stack and Tools

- Languages/frameworks: {{TECH_STACK}}
- Package/build tools: <fill this>
- Test/lint/format tools: <fill this>
- CI/CD: <fill this>

## Collaboration Workflow

1. Context Collection
   - Read `AGENTS.md` first.
   - Read `.agents/KNOWLEDGE_BASE.md` and `.agents/CODING_STYLE.md`.
   - Clarify constraints, acceptance criteria, and affected components.
2. Design (for non-trivial tasks)
   - Use the dedicated `design` skill to draft and iterate design docs.
   - Confirm scope, tradeoffs, and risks before implementation.
3. Coding
   - Implement incrementally and keep code buildable.
   - Follow `.agents/CODING_STYLE.md`.
   - Do not consider backward compatibility unless the task explicitly requires it.
   - Do not add extra protection logic unless explicitly required.
4. Validation
   - Run build -> test -> lint.
   - Use the dedicated review workflow/skill for self-review.
5. Feedback
   - At the end of every task round, add newly identified factual information to `.agents/KNOWLEDGE_BASE.md`.
   - Update `.agents/CODING_STYLE.md` with new conventions.
   - Periodically run `agents-docs-refresh` to clean stale, duplicate, or incorrect entries.
6. Commit
   - For coding tasks, create a commit at the end of each task round.
   - Keep commits atomic and scoped to one logical change.
   - Ensure KB fact updates for the round are included in the same commit.
   - If KB fact updates are missing for the round, do not commit.
   - Include validation notes in commit message or related review notes.

## Project-specific Anti-Patterns

{{ANTI_PATTERNS}}

## Definition of Done

- Requirements and acceptance criteria are met.
- Build, tests, and lint pass (or documented exceptions exist).
- Newly identified factual information is written to `.agents/KNOWLEDGE_BASE.md` every round.
- New conventions are written back to `.agents/CODING_STYLE.md`.
- Risks and open questions are recorded.
- For coding tasks, the round ends with a commit.
- If KB updates are missing, the round is not complete and commit is blocked.
- Implementation follows target-state behavior directly, without compatibility shims or extra protective wrappers.
