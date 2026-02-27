# CODING_STYLE

Use this file for practical conventions that are specific to this project.

## General Principles

- Prefer readability over cleverness.
- Keep changes minimal and scoped.
- Do not preserve backward compatibility by default; implement the target behavior directly.
- Do not add extra protective layers (fallback branches, compatibility shims, dual-write, feature flags) unless explicitly required.

## Naming

- Modules/files: <convention>
- Functions/methods: <convention>
- Variables/constants: <convention>

## Code Structure

- File organization: <convention>
- Error handling: <convention>
- Logging and observability: <convention>

## Testing

- Required test levels: <unit/integration/e2e>
- Test naming: <convention>
- Mocking strategy: <convention>

## Commit Discipline

- For coding tasks, create one commit at the end of each task round.
- Keep each commit focused on one logical change; avoid unrelated edits.
- Every task round must include KB fact capture: add newly identified factual information to `.agents/KNOWLEDGE_BASE.md` before committing.
- Hard gate: if KB fact capture is not completed for the round, commit is not allowed.
- Commit message should explain why and include validation evidence when relevant.

## Knowledge Capture

- Only record factual, verifiable project information in `.agents/KNOWLEDGE_BASE.md`.
- Do not defer KB fact updates to later rounds; capture them in the current round.

## Review Expectations

- Every change includes rationale and validation evidence.
- Non-obvious choices are documented in code or PR description.
