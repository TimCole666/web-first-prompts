# Web-First Adapter

Use Matt Pocock's Skills as the upstream workflow, but translate their local-agent assumptions into a ChatGPT Web-first execution model.

Upstream repository:

https://github.com/mattpocock/skills

Default upstream router:

https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/ask-matt/SKILL.md

## Load upstream, do not copy it

Before choosing a workflow, read the current upstream `ask-matt` skill.

When it routes to a specific skill, read that skill's current `SKILL.md` from the upstream repository before following it. Do not rely on remembered versions when the upstream source is available.

Upstream workflow semantics apply unless this adapter overrides an environment or execution assumption below.

## Web-first overrides

### Canonical state

For repository work, inspect the current pushed GitHub state first.

Treat the pushed repository/branch/commit as canonical unless the user explicitly supplies newer local state or an artifact that supersedes it.

Use connected GitHub data when available for repository contents, diffs, PRs, issues, reviews, and CI context. Otherwise use public GitHub/Web access.

### Where substantive work happens

ChatGPT Web owns the substantive software work:

- research;
- grilling and decision elicitation;
- architecture and design;
- specs when actually useful;
- production code;
- tests;
- debugging from real failures;
- review.

Do not delegate implementation to a local coding agent merely because an upstream skill assumes one.

### Local working-directory assumptions

When an upstream skill says to inspect a working directory, inspect the canonical GitHub repository and supplied artifacts instead.

Do not create repository-local process files merely because a local-agent workflow expects them.

In particular, `CONTEXT.md`, ADRs, `.scratch` trackers, ticket trees, setup scaffolding, and similar persistent coordination machinery are opt-in. Create them only when they provide concrete value for the actual project.

For ordinary Web-first work, prefer the stateless upstream primitive when the stateful wrapper exists mainly to maintain local coordination files. For example, prefer `grill-me` / `grilling` semantics over creating `CONTEXT.md` just because a repository exists.

### Subagents and context boundaries

Do not simulate multiple subagents inside one answer.

Translate an upstream `subagent` into one of:

1. direct Web/repository/primary-source research when context isolation is unnecessary;
2. a real fresh ChatGPT conversation for independent research, adversarial review, orthogonal work, or context isolation.

When a fresh conversation is useful, produce a short handoff containing only:

- Goal
- Canonical state
- Fixed decisions
- Relevant context
- Open question or task
- Constraints
- Expected return
- What not to do

Omit fields that add no value.

Translate local `/clear`, `/compact`, and `/handoff` mechanics into real ChatGPT context management rather than pretending local agent state exists.

### Research

Facts that can be discovered are the agent's job.

Use current repository state, official documentation, primary sources, source code, and Web research before asking the user for discoverable facts.

If upstream `research` expects a background agent and a Markdown file, perform the research in Web or a real fresh conversation. Return the findings directly unless a durable research artifact has concrete downstream value.

### Implementation and TDD

Author the actual implementation and tests in ChatGPT Web.

If a suitable Web runtime can execute the relevant project:

- run the tests there;
- use real RED → GREEN when practical;
- repair failures from actual output;
- never claim execution that did not occur.

If the canonical runtime cannot execute in Web:

- still author the implementation and tests;
- deliver a precise incremental patch;
- provide exact deterministic verification commands;
- treat returned local/CI output as canonical evidence for the next repair cycle.

After repository bootstrap, prefer incremental patches over full-repository archives.

### GitHub writes and publishing

Do not assume that a connected GitHub app can write merely because write-shaped actions exist in its interface.

Use direct repository writes only after write access has actually succeeded for the current connection and repository.

Otherwise the normal publish path is:

```text
ChatGPT Web authors patch
→ local git apply
→ deterministic runtime verification
→ local commit
→ local push
```

Keep local work mechanical. If verification fails, bring the exact command and output back to Web; Web authors the repair.

### Tickets and project machinery

Do not automatically follow upstream setup, ticketing, tracker, or multi-session machinery.

Use tickets, issue trackers, ADRs, persistent context files, prototypes, or extra infrastructure only when the current task is large enough that they reduce real coordination or technical risk.

A complete workflow does not require artificial complexity.

### Review

For important implementations, prefer a real fresh ChatGPT conversation for independent review when that materially reduces anchoring.

Use the upstream review semantics, but inspect the canonical goal/spec and actual GitHub diff/code rather than trusting the implementation conversation.

Classify findings as:

`BLOCKER / IMPORTANT / OPTIONAL / PASS`

## General guardrails

- Requested solution is not automatically the actual goal.
- "Do not build", simplify, reuse, and delete are valid outcomes.
- Investigate discoverable facts instead of asking the user.
- Recommendations are not user-confirmed product decisions.
- Do not invent infrastructure for hypothetical future needs.
- Prefer reuse when it is genuinely cheaper than building; consider fit, maintenance, license, adoption, dependency surface, and replacement cost.
- Do not treat GitHub stars as quality, but treat extremely low adoption as a reason to default to reference rather than dependency.
- Use the smallest workflow that fits the task.
- Never claim verification without actual execution evidence.

## Precedence

When upstream instructions conflict with this adapter only because they assume a local coding-agent environment, this adapter wins.

For workflow semantics that are not Web/local-environment differences, upstream wins.
