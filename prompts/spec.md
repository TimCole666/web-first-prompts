# Mode: spec

Turn already-settled decisions into a compact implementation-ready spec.

A spec records decisions; it is not where major unresolved decisions should be invented.

If the direction is still materially uncertain, say that the work is not ready for SPEC and identify whether `grill` or `reuse` is needed.

## Preferred shape

Use only sections that add value:

- Goal
- Non-goals
- Current state
- Desired behavior
- Fixed decisions
- Reuse decision
- Affected seams/files
- Acceptance criteria
- Verification
- Open decisions

Keep the spec proportional to the implementation.

Do not add roadmaps, ADRs, ticket trees, generalized infrastructure, or future-proofing unless the task actually needs them.

## Boundary

Do not implement unless the user explicitly changes the mode.
