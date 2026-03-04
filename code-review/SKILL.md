---
name: code-review
description: Perform critical Linus-style code review on selected git diff target and save a markdown report with file-line findings and action items.
license: MIT
compatibility: opencode
metadata:
  audience: engineers
  workflow: review
---

# Code Review

## What I do
- Let the user choose exactly one review target:
  1. Current uncommitted diff in working tree (staged + unstaged)
  2. All changes on current branch vs a base branch (e.g. `main`/`master`/`dev`), including committed and uncommitted changes
  3. A user-provided diff text
- Apply **Linus-style critical thinking** for deep review from macro design to micro implementation.
- Produce an actionable markdown report including:
  - Intent and context understanding
  - Risk and impact analysis
  - File-and-line-level findings
  - Recommendations and action items

## When to use me
- Before PR/merge when high-quality review is required.
- For architecture, concurrency, performance, or reliability-sensitive changes.
- When you need a standardized review report saved to disk.

## Inputs
- `review_target` (required): `working_tree` | `branch_vs_base` | `provided_diff`
- `base_branch` (optional): required when `review_target=branch_vs_base`, e.g. `main`
- `provided_diff` (optional): required when `review_target=provided_diff`
- `context_notes` (optional): feature context, ticket links, constraints, regression history
- `output_path` (optional): default `reviews/code-review-<timestamp>.md`
- `template_path` (optional): default `skills/code-review/REVIEW_TEMPLATE.md`
- `severity_rule` (optional): default `critical/high/medium/low`

## Target selection and diff collection
1. Confirm the selected target first. If unclear, ask follow-up questions and do not guess.
2. Collect diff by target:

### A) Uncommitted working tree diff
- Use:
  - `git diff -- .` (unstaged)
  - `git diff --cached -- .` (staged)
- Merge both as one review input and label their source.

### B) Current branch vs base branch
- Verify `base_branch` exists and is comparable.
- Use three-dot diff and include committed + uncommitted changes:
  - Committed: `git diff <base_branch>...HEAD -- .`
  - Uncommitted: same staged/unstaged commands as A
- Explain merge-base semantics to avoid interpretation errors.

### C) User-provided diff
- Use the provided diff directly.
- If file path or line context is missing, state that review precision is reduced.

## Review process (macro → micro)
1. **Understand intent and context**
   - What problem does this change solve?
   - Why this approach over alternatives?
   - What defines success?
2. **Macro review**
   - Architecture and module boundaries
   - Separation of concerns and coupling risks
   - Backward compatibility, migration, rollback strategy
3. **Micro review**
   - Correctness: edge cases, null handling, error handling, state transitions
   - Concurrency: race conditions, deadlocks, visibility, atomicity
   - Resource safety: memory/handle/connection/thread leaks
   - Performance: complexity, hot paths, IO amplification, lock contention, cache behavior
   - Security: injection, privilege escalation, sensitive data handling, dependency risk
   - Maintainability: naming, comments, readability, duplication, function size
   - Consistency: linting, formatting, project conventions
4. **Test coverage review**
   - Main path, failure path, edge case, and regression coverage
   - Missing concurrency tests, benchmarks, or integration tests
   - Whether tests validate behavior instead of implementation details
5. **Use the standard template output**
   - Default to `skills/code-review/REVIEW_TEMPLATE.md`
   - Keep section structure intact and fill with review evidence
6. **Evidence-based reporting**
   - Every finding should include file path and line range whenever possible
   - Include severity, trigger conditions, impact, and fix recommendation

## Review checklist
- [ ] Is the change purpose and business context clear?
- [ ] Is the design minimal, coherent, and evolvable?
- [ ] Are module boundaries clean without unnecessary coupling?
- [ ] Is correctness solid for edge/error/rollback paths?
- [ ] Are there no race/deadlock/starvation risks?
- [ ] Is there no memory/connection/file-handle leak risk?
- [ ] Is complexity and critical-path performance acceptable?
- [ ] Are security controls and data handling adequate?
- [ ] Is test coverage sufficient (unit/integration/regression/concurrency/perf)?
- [ ] Are readability and code consistency acceptable?
- [ ] Is operability adequate (logging/monitoring/alerting/debuggability)?
- [ ] Are compatibility, migration, and rollback risks addressed?

## Output format
- Use `skills/code-review/REVIEW_TEMPLATE.md` as the single source of truth for report structure.
- Save the filled report to `output_path`.

## Severity guideline
- `critical`: can cause data corruption, severe security impact, or production outage; block merge.
- `high`: likely functional bugs, concurrency hazards, or major performance regression; fix before merge.
- `medium`: maintainability or edge-case issues; fix soon.
- `low`: style or minor improvement suggestions.

## Guardrails
- Do not stop at superficial style comments; prioritize design and correctness first.
- Do not hallucinate missing code; mark unknowns as “needs more context”.
- Do not report untraceable issues; provide file and line references whenever possible.
- If diff is large, provide high-risk summary first, then deep-dive in batches.

## Example invocation
```text
skill({
  name: "code-review",
  review_target: "branch_vs_base",
  base_branch: "main",
  context_notes: "Refactor payment settlement flow to reduce timeout rate",
  output_path: "reviews/payment-refactor-review.md"
})
```
