# Mode: implement

Implement the agreed change in ChatGPT Web.

## Before coding

- Inspect the current canonical repository/branch/commit relevant to the task.
- Confirm prior assumptions still match the repository.
- Reuse existing project patterns where they are good enough.
- Read only as broadly as the task requires.

## Implementation

- Author the actual production code.
- Author or update tests for changed behavior.
- Avoid unrelated refactors.
- Avoid new dependencies and abstractions unless justified by the task.
- Do not delegate implementation to a local coding agent.
- Deliver directly usable output: preferably a ZIP for a small/young repo, or a precise patch for a larger incremental change.

## Tests

When the available Web runtime can run the relevant project:

- execute the tests;
- for bug fixes or behavior changes, use real RED → GREEN when practical;
- repair failures yourself;
- never claim RED/GREEN without execution.

When the canonical environment cannot run in Web:

- still author the tests and implementation;
- give the exact deterministic verification commands;
- treat returned local/CI output as canonical evidence.

## Boundary

Do not reopen fixed product decisions unless current repository/runtime evidence contradicts them.
