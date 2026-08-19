# Web-First Adapter

A thin compatibility layer for using [Matt Pocock's Skills](https://github.com/mattpocock/skills) from ChatGPT Web.

This repository does **not** reimplement Matt's workflow. It keeps only the differences required by a Web-first development model.

## Start

In a fresh ChatGPT Web conversation:

```text
Follow this adapter:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/web-first.md

Task:
<your real software task>
```

The adapter loads the current upstream workflow from Matt's repository and applies the Web-first overrides.

## Operating model

```text
GitHub canonical state
        ↓
ChatGPT Web
inspect / research / grill / design / implement / review
        ↓
Web runtime verification when available
        ↓
patch + deterministic verification commands when needed
        ↓
local git apply / runtime verify / commit / push
```

The standard GitHub connection is useful for reading canonical repository state, diffs, PRs, issues, and CI context. Do not assume repository write access unless a write action has actually been demonstrated for that connection.

A local coding agent is not part of the default workflow.

## Upstream policy

Matt's current `main` branch is the upstream workflow definition.

We intentionally follow upstream raw skills rather than copying them here. If upstream changes materially, the adapter should be reviewed rather than maintaining a parallel fork of the workflow.

## What this repository owns

Only Web-first differences, especially:

- GitHub/connected sources instead of assuming a local working directory;
- ChatGPT Web as the substantive development environment;
- real fresh ChatGPT conversations instead of simulated local subagents;
- no automatic `CONTEXT.md`, ADR, ticket, or setup machinery unless it earns its place;
- Web-native tests when possible, deterministic local/CI verification otherwise;
- patches as the normal handoff when direct repository writes are unavailable;
- local `git` limited to applying, verifying, committing, and pushing changes.

See [`web-first.md`](web-first.md) for the actual adapter.

## License

MIT.
