---
name: skill-update
description: Improve an existing skill using real usage feedback, failures, and observed gaps.
---

# Skill Update

## What I do
- Update an existing skill based on real usage evidence.
- Turn vague feedback into concrete edits in `SKILL.md`.
- Improve trigger clarity, constraints, steps, and output format.
- Keep the skill concise while increasing reliability and reuse.

## When to use me
- A skill works but often gives inconsistent results.
- Users report repeated misunderstandings or missing edge-case handling.
- You have post-usage insights from PR review, incident notes, or task transcripts.

## Inputs
- `skill_name` (required): target skill directory name.
- `evidence` (required): observed issues, examples, logs, or user feedback.
- `goal` (optional): `clarity` | `coverage` | `safety` | `performance` | `all` (default: `all`).
- `strictness` (optional): `minimal` | `balanced` | `aggressive` (default: `balanced`).
- `base_path` (optional): skill root path. Default: `.agents/skills`.

## Steps
1. Locate `<base_path>/<skill_name>/SKILL.md` and read it fully.
2. Normalize `evidence` into actionable findings (ambiguity, missing constraints, weak examples, noisy steps, etc.).
3. Classify each finding as one of:
   - trigger problem
   - input contract problem
   - process/step problem
   - output format problem
   - safety/guardrail problem
4. Propose edits that are specific, testable, and minimally invasive.
5. Update `SKILL.md` sections in this order:
   - `When to use me`
   - `Inputs`
   - `Steps`
   - `Output` / output format examples
   - `Guardrails` / `Troubleshooting` (if present)
6. Remove outdated, duplicate, or contradictory statements.
7. Produce a short changelog summary with:
   - what changed
   - why it changed
   - what behavior should improve

## Output
- Updated `<base_path>/<skill_name>/SKILL.md`.
- A concise change summary suitable for commit message or PR description.

## Example Invocation
```text
skill({
  name: "skill-update",
  skill_name: "code-review",
  goal: "clarity",
  evidence: [
    "Users forget to specify review target and the skill asks too late",
    "Report output sometimes misses file-line references"
  ]
})
```
