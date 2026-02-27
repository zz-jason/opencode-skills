---
name: agents-docs-refresh
description: Rewrite .agents knowledge docs to remove incorrect, outdated, and duplicate content.
compatibility: opencode
metadata:
  audience: project-maintainers
  workflow: knowledge-hygiene
---

# Refresh Agent Docs

## What I do
- Audit `.agents/KNOWLEDGE_BASE.md` and `.agents/CODING_STYLE.md` for correctness and usefulness.
- Remove or rewrite content that is incorrect, outdated, vague, or duplicated.
- Consolidate overlapping entries into canonical, concise sections.
- Classify and reorganize all retained knowledge into stable categories for faster retrieval.
- Preserve useful project facts and actionable conventions.

## When to use me
- Periodic maintenance (for example, weekly or after major refactors).
- You notice contradictions across historical notes.
- The docs have become noisy and agents are producing lower-quality results.

## Inputs
- `target_path` (optional): repository root path. Default: `.`.
- `scope` (optional): `knowledge`, `style`, or `both`. Default: `both`.
- `strictness` (optional): `conservative`, `balanced`, or `aggressive`. Default: `balanced`.
- `preserve_sections` (optional): section names that must remain.
- `knowledge_taxonomy` (optional): custom KB categories in desired order. Uses default taxonomy when omitted.
- `report` (optional): whether to output a brief cleanup report. Default: `true`.

## Steps
1. Read `AGENTS.md`, `.agents/KNOWLEDGE_BASE.md`, and `.agents/CODING_STYLE.md`.
2. Classify each entry as `keep`, `rewrite`, `merge`, or `remove`.
3. For KB entries marked `keep/rewrite/merge`, assign category labels and regroup them by taxonomy.
4. Remove outdated and duplicate content; rewrite ambiguous statements into testable guidance.
5. Normalize structure and wording while keeping project-specific details.
6. Validate that both docs remain practical, concise, and internally consistent.
7. If `report=true`, output a short summary of removed, merged, rewritten, and re-categorized items.

## Default KB taxonomy
- `System Overview`: architecture, core modules, and data flows.
- `Invariants and Constraints`: hard rules, compatibility limits, and safety boundaries.
- `Decisions and Rationale`: important decisions with reasons and impacts.
- `Pitfalls and Failure Modes`: common mistakes, edge cases, and recovery hints.
- `Operational Knowledge`: setup, build/test, deploy, and runtime notes.
- `Glossary`: domain-specific terms and definitions.

## Output
- Cleaned `.agents/KNOWLEDGE_BASE.md` and/or `.agents/CODING_STYLE.md` with higher signal, lower noise, and categorized KB content.

## Example Invocation
```text
skill({ name: "agents-docs-refresh" })
```

## Troubleshooting
- Missing files: run `agents-docs-init` first.
- Over-pruned content: rerun with `strictness=conservative`.
- Persistent contradictions: resolve source-of-truth in code/tests, then rerun cleanup.
