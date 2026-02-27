---
name: design
description: Write design documents using the bundled design template.
compatibility: opencode
metadata:
  audience: developers
  workflow: design-doc
---

# Design

## What I do
- Draft a complete design document from the bundled template at `DESIGN_TEMPLATE.md`.
- Turn a feature request into a review-ready proposal with clear tradeoffs and open questions.

## When to use me
- You need a new design proposal before implementation.
- You want design docs to stay consistent with the expected structure.

## Inputs
- `topic` (required): feature or problem being designed.
- `output_path` (optional): destination markdown path. Default: `docs/design/<topic>.md`.
- `authors` (optional): co-author line content for the template.
- `date` (optional): date string in `YYYY-MM-DD` format.
- `constraints` (optional): non-functional requirements, compatibility limits, or scope boundaries.

## Steps
1. Read `DESIGN_TEMPLATE.md` and preserve its section structure.
2. Collect context: motivation, users, and concrete use cases.
3. Fill each section with project-specific details:
   - `Summary`: one-paragraph proposal overview.
   - `Background`: problem, motivation, and scenarios.
   - `Detailed Design`: architecture, APIs, algorithms, data flow, and corner cases.
   - `Alternative Designs Considered`: options explored and why rejected.
   - `Unresolved Questions`: decisions still pending.
4. Keep claims testable and implementation-oriented; avoid vague statements.
5. Emit the final markdown file at `output_path`.

## Output
- A review-ready design document that follows `DESIGN_TEMPLATE.md` exactly, with project-specific content filled in.

## Example Invocation
```text
skill({ name: "design" })
```

## Troubleshooting
- Missing sections: re-open `DESIGN_TEMPLATE.md` and ensure all headings are present.
- Low review quality: add concrete APIs, pseudocode, and failure cases in `Detailed Design`.
- Scope creep: move uncertain items to `Unresolved Questions` and keep the core proposal minimal.
