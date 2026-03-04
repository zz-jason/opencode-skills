---
name: hierarchical-orchestration
description: Execute complex tasks with a coarse-grained worker DAG and adaptive sub-workers under strict budget and handoff controls.
license: MIT
compatibility: opencode
metadata:
  audience: opencode-agents
  workflow: multi-agent-planning
---

# Hierarchical Orchestration

## What I do

- Turn a complex goal into a high-cohesion, low-coupling worker DAG.
- Default to single-worker execution and only split when measured benefit exceeds handoff cost.
- Allow each worker to adaptively spawn short-lived sub-workers when local scope explodes.
- Enforce structured contracts so outputs are machine-readable and retryable.

## When to use me

- The user asks for multi-step work that may need planning, execution, verification, and iterative fixes.
- The task may benefit from parallel exploration or role specialization.
- You need a repeatable master/worker execution policy instead of ad-hoc decomposition.

## Inputs

- `goal` (required): what must be delivered.
- `constraints` (optional): scope limits, security constraints, forbidden actions.
- `acceptance_criteria` (optional): explicit done conditions.
- `budget` (optional):
  - `max_time_minutes`
  - `max_parallel_workers`
  - `max_subworker_depth` (default `1`)
  - `max_retries_per_node` (default `2`)
  - `max_handoffs`
- `context_sources` (optional): docs, code paths, tickets, prior artifacts.

## Core policies

### 1) SplitPolicy (when to split)

Use these signals to decide whether to move from single-worker to DAG:

- `parallelism_potential >= 2` independent subproblems.
- `tool_heterogeneity >= 2` major tool domains (for example code + browser + db).
- `uncertainty >= medium` and an exploration phase can reduce risk.
- `retry_isolation_value >= medium` (local retries are cheaper than full reruns).

Prefer **single-worker** when:

- The task can likely finish in one coherent context window.
- Strong sequential dependency means splitting adds handoff overhead.
- Context transfer size is high and would cause expensive shuffle.

### 2) MergePolicy (when to merge)

If planned fan-out has high handoff cost, merge neighboring nodes:

- Merge if expected handoff payload is large and boundaries are weak.
- Merge if two nodes repeatedly exchange natural-language context instead of artifacts.
- Merge if estimated split gain is smaller than startup + coordination overhead.

### 3) BudgetPolicy

- Honor hard caps from `budget`.
- Never exceed `max_subworker_depth`.
- If budget pressure is high, degrade to fewer workers or single-worker mode.
- Stop fan-out when marginal gain is low.

### 4) RetryPolicy

- Retry at node level, not full workflow level.
- Cap retries per node via `max_retries_per_node`.
- After retry cap, return partial result + blocker report + recommended human decision.

### 5) ContractSchema (worker I/O)

Every worker must produce structured output:

```json
{
  "status": "success|failed|partial",
  "summary": "one short paragraph",
  "artifacts": [
    { "type": "diff|report|plan|log|link", "path": "string", "notes": "string" }
  ],
  "findings": [
    { "severity": "low|medium|high", "message": "string", "evidence": "string" }
  ],
  "next_actions": ["string"],
  "confidence": 0.0
}
```

Use artifacts as edge payloads between workers. Avoid passing long free-form prose.

## Recommended execution shape

### Default for coding tasks

- `BuildWorker` (implement + tests in one context).
- `VerifyWorker` (independent lint/typecheck/test and root-cause report).
- `FixWorker` (only on failure; loops back to verify).

Workflow:

```text
BuildWorker -> VerifyWorker
                 |
               fail
                 v
             FixWorker --(retry)-> VerifyWorker
```

### Adaptive expansion inside a worker

If a worker detects scope explosion, it may spawn sub-workers:

- Trigger examples: too many touched files, too many modules, low confidence after initial scan.
- Sub-workers should be short-lived and research-heavy.
- Merge back into a single synthesized artifact before continuing.

## Step-by-step behavior

1. Normalize the request into `goal`, `constraints`, `acceptance_criteria`, and `budget`.
2. Score task shape (`uncertainty`, `parallelism_potential`, `handoff_cost`, `tool_heterogeneity`).
3. Choose execution mode:
   - Single-worker first, unless split signals clearly dominate.
4. Build coarse DAG with minimal nodes and explicit artifact boundaries.
5. Execute nodes with structured outputs and per-node retries.
6. Allow adaptive sub-worker fan-out within budget limits.
7. Merge outputs, run final verification, and report outcomes with artifacts.

## Output

- A concise plan decision (`single-worker` or `multi-worker DAG`) and why.
- Execution trace: node statuses, retries, budget use, and key artifacts.
- Final deliverable mapped to acceptance criteria.

## Safety notes

- Do not split for novelty. Split only when measurable benefit exists.
- Keep decomposition shallow. Default max sub-worker depth is 1.
- Preserve deterministic checkpoints so failed nodes can be replayed.
- Respect tool and permission boundaries from the host environment.

## Example Invocation

```text
skill({ name: "hierarchical-orchestration" })
```
