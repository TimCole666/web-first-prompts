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

### Deterministic local handoffs

When the local side is assigned deterministic mechanics, the final handoff must be executable without the user making design, naming, policy, or target-selection decisions while editing commands.

Before emitting a final local command block:

1. resolve values that can be discovered from canonical state, connected tools, authenticated context, existing remotes, or deterministic tool output;
2. reuse values the user already explicitly confirmed; do not ask for them again;
3. identify only the remaining user-owned values that materially change the target or result;
4. ask for those values, give recommendations when useful, and wait for confirmation;
5. only then emit the exact commands.

Final deterministic handoffs must contain **zero user-editable semantic placeholders**, including forms such as:

```text
<OWNER>
<REPO>
<BRANCH>
<PATH>
YOUR_USERNAME
CHOOSE_PUBLIC_OR_PRIVATE
```

Runtime shell variables are allowed when they are populated mechanically and do not hide a user-owned choice, for example `ROOT="$(pwd)"`.

If a required fact is only discoverable from the local environment, first give an exact discovery command and ask the user to return its output. That discovery step is mechanical; use its result before producing the final action block.

A recommendation is not a resolved command parameter.

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

### Spec decisions and canonicalization

Follow the current upstream `to-spec` semantics rather than treating SPEC as free-form prose.

Upstream already requires proposed testing seams to be checked with the user. Treat that check as a hard gate, not a suggestion.

Do not reopen the full grill or turn SPEC into an implementation questionnaire. Ordinary internal implementation choices remain the agent's responsibility.

A proposed architecture decision requires targeted user confirmation before it becomes `Fixed` in the canonical spec when all of these are true:

1. it creates or changes a primary application, service, runtime, integration, state-ownership, or testing seam;
2. it materially constrains module architecture, testing boundaries, implementation decomposition, or later replacement cost;
3. the canonical repository and prior user-confirmed decisions do not already establish it;
4. the user has not explicitly delegated that class of architecture decision.

When this gate is triggered, pause only that branch and present:

```text
Recommended primary seam:
...

Why:
...

Alternatives materially considered:
...

This decision will determine:
...

Confirm / modify?
```

A recommendation remains a recommendation until confirmed or delegated.

For multi-session or repository-backed work, distinguish:

- `SPEC DRAFTED` — the content exists only in conversation or an unpersisted artifact;
- `SPEC COMPLETE` — the final spec is persisted in canonical repository state and the publish step has succeeded.

A spec intended to govern later fresh sessions is not complete while it exists only in chat.

#### Existing canonical repository

Before creating a new spec file, inspect the repository for an existing canonical spec/design convention.

- If a relevant canonical spec/design artifact already exists, update it.
- If the repository has an established docs/spec convention, follow it.
- Otherwise, for one active project-level spec, default to `SPEC.md` at the repository root.
- Do not create a documentation hierarchy merely to satisfy the workflow.

ChatGPT Web owns the path decision, exact spec contents, and patch.

When direct repository writes have not been proven for the current connection, complete persistence mechanically:

```text
ChatGPT Web authors final spec patch
→ local git apply
→ git diff --check
→ inspect git diff / git status
→ local commit
→ local push
→ SPEC COMPLETE
```

If deterministic checks expose a problem, Web authors the repair. Do not use a local coding agent to rewrite or reinterpret the spec.

Do not announce `SPEC COMPLETE` until the push (or an equivalent proven direct repository write) has succeeded.

#### Greenfield with no canonical repository

If the work is a real multi-session repository-backed build and no canonical repository exists yet, SPEC owns a thin canonicalization transition.

ChatGPT Web should:

1. finalize any required high-impact seam confirmations;
2. choose the minimal bootstrap contents;
3. author the bootstrap artifact, including the final canonical spec;
4. resolve the canonical repository-creation parameters required for the first push;
5. provide exact deterministic repository initialization / validation / commit / push mechanics.

Before the final repository-creation commands, inspect any reliably discoverable GitHub identity, organization context, and existing remote information.

Treat discovery and choice separately:

- an authenticated GitHub identity is a discovered candidate, not automatically the intended repository owner;
- use an owner without asking only when prior user-confirmed context already establishes that owner, or the user explicitly delegated repository placement;
- confirm the repository name unless the user already supplied an exact repository/project name that clearly serves as the repo name, or explicitly delegated naming;
- confirm visibility unless `public` / `private` was already explicitly fixed or delegated;
- ask about description, license, branch naming, organization policy, or other bootstrap options only when they materially matter to this repository.

Ask only the unresolved values. Do not turn bootstrap into a generic GitHub questionnaire.

After those values are resolved, the final creation/push commands must obey the deterministic local handoff invariant above: no `<OWNER>`, `<REPO>`, visibility toggle, or other user-editable semantic placeholder may remain.

With no established repository convention, default the canonical spec to `SPEC.md`.

The bootstrap may include only files that are already justified at this phase. Do not add implementation, `CONTEXT.md`, ADRs, tickets, roadmap files, or planning infrastructure merely to make the repository look complete.

Do not create an empty repository and expect a later agent to reconstruct the spec from conversation history.

After the first successful push, that GitHub state becomes canonical and later fresh sessions must read it first.

#### Leaving SPEC

After canonical persistence succeeds:

- if the implementation is multi-session, important, or benefits materially from context isolation, generate a short fresh-conversation IMPLEMENT handoff that references the canonical repository and spec instead of duplicating them;
- if the implementation is genuinely small and the current context remains healthy, continuing in the same conversation is allowed;
- do not force `to-tickets` or other project-management machinery unless the work actually needs it.

The Web-first phase boundary is therefore:

```text
GRILL or other upstream entry
→ to-spec synthesis
→ targeted high-impact seam confirmation when required
→ final spec
→ resolve any user-owned canonicalization parameters
→ exact zero-placeholder persistence handoff
→ deterministic verification / diff inspection
→ commit + push
→ verify pushed canonical state
→ SPEC COMPLETE
→ IMPLEMENT (fresh conversation when appropriate)
```

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
- Recommendations are not user-confirmed decisions; high-impact canonical seams require the targeted confirmation gate above unless already established or delegated.
- Do not invent infrastructure for hypothetical future needs.
- Prefer reuse when it is genuinely cheaper than building; consider fit, maintenance, license, adoption, dependency surface, and replacement cost.
- Do not treat GitHub stars as quality, but treat extremely low adoption as a reason to default to reference rather than dependency.
- Use the smallest workflow that fits the task.
- Never claim verification without actual execution evidence.
- Never hand off user-editable semantic placeholders when the local role is deterministic execution.
- Never claim a repository-backed SPEC phase is complete before its canonical persistence step succeeds.

## Precedence

When upstream instructions conflict with this adapter only because they assume a local coding-agent environment, this adapter wins.

For workflow semantics that are not Web/local-environment differences, upstream wins.
