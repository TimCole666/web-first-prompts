# Web-First Prompts

A tiny GitHub-hosted prompt registry for Web-first software development.

No CLI. No skill installer. No local agent required.

## Quick use

In a fresh ChatGPT Web conversation, send:

```text
Use this prompt library: https://github.com/YOUR_NAME/web-first-prompts
Mode: review

Goal: review the current implementation
Canonical state: https://github.com/ORG/REPO/tree/BRANCH
Relevant context: ...
```

The assistant should read this repository, load the matching file under `prompts/`, and follow it.

You only need to remember one stable repository URL plus a mode.

## Modes

| Mode | File | Purpose |
|---|---|---|
| `main` | `prompts/main.md` | Coordinator / router |
| `grill` | `prompts/grill.md` | Challenge the goal and proposed solution |
| `reuse` | `prompts/reuse.md` | Search GitHub and primary sources before building |
| `spec` | `prompts/spec.md` | Turn settled decisions into a compact implementation-ready spec |
| `implement` | `prompts/implement.md` | Web authors the actual implementation and tests |
| `review` | `prompts/review.md` | Fresh independent review |
| `repair` | `prompts/repair.md` | Repair from real verification failures |
| `checkpoint` | `prompts/checkpoint.md` | Compact a long coordinator conversation |

## Handoffs

The `main` mode should not paste full worker prompts.

When a fresh conversation is useful, it should generate a short launcher that references this same prompt repository:

```text
Use this prompt library: <same repo URL>
Mode: <worker mode>

Goal: ...
Canonical state: ...
Fixed decisions: ...
Relevant context: ...
Task: ...
Constraints: ...
Expected return: ...
Do not: ...
```

Delete fields that are unnecessary for the task.

## Operating model

```text
GitHub / project files
        ↓
ChatGPT Web
research / grill / spec / code / tests / review
        ↓
ZIP / patch / complete code
        ↓
deterministic local or CI verification
        ↓
manual commit / push
```

If ChatGPT Web has a suitable runtime, run tests there too. Never claim RED, GREEN, or verification without actual execution.

## Design rules

- Use the smallest workflow that fits the task.
- Requested solution is not automatically the real goal.
- New infrastructure defaults to `grill → reuse → spec → implement`.
- Small bugs and mechanical changes do not need ceremony.
- Prefer primary sources and current GitHub source when external research matters.
- Low-adoption projects can be useful references without being good dependencies.
- Do not invent local coding-agent work when Web can author the result directly.
- Real fresh conversations are the subagent mechanism; do not simulate multiple subagents inside one response.
- GitHub/pushed repository state is canonical unless the user explicitly supplies newer local state.

## License

MIT.
