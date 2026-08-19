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

### Artifact verification

For repository-backed incremental patches, generate and verify the patch against the exact canonical base whenever that base is accessible.

Before delivering a patch:

1. read the canonical repository state at a specific branch / commit;
2. generate the patch from that exact content;
3. run `git apply --check` against that exact base;
4. apply it in a disposable verification tree and run `git diff --check`;
5. only then describe the patch as applicability-verified.

A synthetic, partial, or reconstructed base may be useful for debugging, but it is not final applicability evidence unless its exact content identity has first been proven against the canonical repository (for example by matching the canonical Git blob SHA).

If the exact canonical base is genuinely unavailable, say that applicability verification is provisional and make the local `git apply --check` authoritative.

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

Do not treat a Git clone inside the Web sandbox as the default way to inspect or materialize a repository.

When canonical repository contents are available through the connected GitHub capability or public GitHub fetch, use that source directly. Do not first attempt `git clone`, `git fetch`, or `git pull` merely to discover whether sandbox network access works.

If filesystem mechanics are needed for patch generation or verification, materialize the exact required canonical files into a disposable Web working tree and use Git locally there. Use an existing trustworthy Web working tree when one is already available; do not provision one through network cloning merely because an upstream local-agent workflow assumes a checkout.

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

Preserve upstream main-flow context hygiene: keep `GRILL → SPEC → TO-TICKETS` in one continuous Web conversation through `TICKETS COMPLETE` when practical. Do not insert fresh chats or `/handoff` between those phases merely because the work is repository-backed. If context quality approaches the smart-zone limit, recover at the nearest phase boundary rather than pushing on with degraded context.

Use upstream `/handoff` semantics narrowly for portability: a new harness, a new repository/directory, a colleague, or a side-task fork. Do not use a full handoff as the default transition between canonical implementation tickets.

When a genuine portability handoff is useful, produce a short handoff containing only:

- Goal
- Canonical state
- Fixed decisions
- Relevant context
- Open question or task
- Constraints
- Expected return
- What not to do

Omit fields that add no value.

After `TICKETS COMPLETE`, start each canonical ticket in a fresh IMPLEMENT conversation with a thin self-locating dispatch rather than a handoff summary. The dispatch must identify:

- Web-first workflow: `https://github.com/TimCole666/web-first-prompts`, adapter `web-first.md`; read current `main` before substantive work unless the user explicitly pins a workflow revision;
- canonical product repository and branch;
- canonical spec path or URL;
- selected canonical ticket by stable path or tracker ID/URL.

Tell the fresh IMPLEMENT conversation to inspect those canonical sources before making judgments. Do not copy canonical workflow rules, spec or issue contents, or diffs into the dispatch when they can be retrieved canonically; reference them by locator or commit instead. Include only session-local findings or constraints that are not already recorded elsewhere and materially matter to the selected ticket.

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

After canonical persistence succeeds, preserve the upstream multi-session branch instead of jumping directly to implementation.

First decide whether the implementation is genuinely multi-session.

- If **yes**, `to-tickets` is the default next stage. Run the current upstream `to-tickets` skill against the canonical spec before any implementation session starts.
- If **no**, implementation may continue directly; use a fresh conversation only when it materially improves context quality or independence.
- If work was already classified as multi-session, do not silently bypass `to-tickets`. Skip it only if new evidence shows the work now fits one implementation session, or the user explicitly chooses to bypass decomposition.

For a multi-session build, keep the planning chain in the same Web conversation through ticket publication and `TICKETS COMPLETE` unless context quality forces recovery at a phase boundary. Fresh IMPLEMENT conversations begin only after canonical tickets exist.

1. derive the ticket breakdown from the canonical spec and current canonical repository, not from an unpushed implementation attempt;
2. follow upstream tracer-bullet rules: each ticket is a narrow but complete vertical slice, independently demoable or verifiable, and sized for one fresh context window;
3. declare only real blocking edges;
4. present the proposed breakdown and get the user's granularity / dependency confirmation as upstream requires;
5. select or preserve the canonical tracker backend using the tracker rules below; do not let the available write transport choose it;
6. publish the approved tickets to that tracker;
7. after `TICKETS COMPLETE`, start implementation from the current ticket frontier, one canonical ticket per fresh IMPLEMENT conversation.

Do not let `/implement` invent a conversation-local "first slice" when canonical execution tickets are required.

The Web-first phase boundary for a genuine multi-session build is therefore:

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
→ to-tickets
→ user confirms ticket granularity / blocking edges
→ select or preserve canonical tracker backend
→ publish canonical tickets through that tracker
→ TICKETS COMPLETE
→ IMPLEMENT one frontier ticket per fresh conversation
```

For work that genuinely fits one implementation session:

```text
... → SPEC COMPLETE → IMPLEMENT
```

### Implementation and TDD

ChatGPT Web owns test design, test code, production code, and repairs from test or verification failures.

Use the current upstream TDD semantics where TDD is valuable: work at pre-agreed seams, observe a real RED before writing the minimal GREEN change, and proceed one vertical slice at a time. Do not describe a cycle as TDD unless both RED and GREEN were actually observed.

Treat research, authorship, and execution as separate concerns. Continue to use Web search, official documentation, primary sources, and repository/source inspection proactively when implementation facts are uncertain.

Before deciding where to execute, cheaply probe the existing Web development environment for the required runtime, toolchain, and already-available dependencies.

- If the required environment is already available at negligible setup cost, Web may run targeted tests, typechecking, or builds for fast implementation feedback. Real RED → GREEN observed there is valid development evidence.
- Do not download, install, or provision a missing substantial project runtime, engine, toolchain, service, or dependency environment merely to make Web-side execution possible.
- If Web execution would require that provisioning, stop at authorship and use the project's real local or CI runtime for execution instead.

For repository-backed work, treat the project's canonical local/CI runtime as the authoritative verification environment unless the repository already establishes another one. Web-side execution is opportunistic feedback, not a reason to replace required project-runtime verification.

When a valuable TDD slice cannot execute cheaply in Web:

```text
Web authors one RED test
→ local/CI mechanically applies or runs the exact command
→ return the real output
→ Web confirms the expected RED and authors the minimal GREEN change
→ local/CI reruns the same focused verification
→ repeat only while the next TDD slice is worth the round trip
```

Keep the local role mechanical: it does not design tests, change implementation, or diagnose failures. Returned execution output is evidence for ChatGPT Web, which owns diagnosis and repair.

Do not force TDD where its round-trip cost exceeds its value. Preserve upstream `implement`'s "where possible" posture and concentrate TDD on pre-agreed, behaviorally important seams. Other implementation may be authored with appropriate tests and verified normally.

Before completing repository-backed work, run the required project verification in the authoritative local/CI runtime even when Web-side checks already passed. Never claim execution that did not occur.

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

Do not create process artifacts for ceremony. This does **not** make upstream `to-tickets` optional when a genuine multi-session build needs canonical implementation-sized work units.

Use `to-tickets` by default when the build has already been classified as multi-session: implementation requires multiple fresh work sessions, and vertical slices / blocking edges provide real execution or coordination value.

Tickets have real execution value when they:

- define independently implementable and verifiable vertical slices;
- fit one fresh implementation context each;
- map back to the canonical spec without duplicating it;
- preserve real blocking relationships across sessions;
- give `/implement` a canonical work unit instead of asking it to invent scope.

Tickets are ceremony when they merely split a one-session change, divide work by helper/layer rather than observable behaviour, or duplicate the spec without improving execution.

Treat **tracker selection** and **write transport** as separate decisions.

Follow the project's existing canonical tracker convention when one exists. When no tracker convention is already established:

- For a canonical GitHub repository whose genuine multi-session work benefits from stable ticket IDs, blocker/dependency state, open/closed state, or fresh-conversation coordination, default to **GitHub Issues** unless the user or project has already chosen otherwise. This preserves upstream setup's default posture for a GitHub remote.
- Prefer upstream **local Markdown** when no real tracker is configured or desired and local repository state is sufficient for the work. Use the upstream per-ticket layout `.scratch/<feature-slug>/issues/<NN>-<slug>.md`, one file per ticket, with its current status/blocker semantics. Do not invent a combined `docs/tickets.md` or another tracker shape unless the project explicitly chooses it.
- For another already-selected real tracker, preserve that tracker and follow its configured workflow.

A connector permission or capability failure is a transport failure, not a tracker-selection signal. **Never silently change the selected tracker backend because one write path failed.**

For GitHub Issues, publish and maintain the selected tracker with this fallback order:

1. if the connected Web GitHub capability can perform the required issue write, use it directly;
2. otherwise, when authenticated local `gh` is available, ChatGPT Web authors an exact zero-placeholder `gh` command/script and local execution performs only the mechanics;
3. if neither path can write the selected GitHub tracker, report the capability blocker. Change tracker backend only after an explicit user decision when that would change canonical execution-work semantics.

A deterministic `gh` fallback must keep ticket authorship in Web: repository identity, titles, bodies, acceptance criteria, and blocker relationships are already fixed. It should create blockers first, capture the real issue identifiers mechanically, use those identifiers for dependency references, guard against obvious duplicate creation before the first write, and return the created issue URLs/numbers for verification. Runtime-generated issue numbers are execution results, not user-editable placeholders.

Apply the same transport fallback to later tracker lifecycle writes when the workflow calls for them: claim/assignment, labels, comments, dependency updates, close/reopen, or other status changes. A connector becoming read-only later does not make the tracker non-canonical.

If GitHub Issues are selected, canonical state is split deliberately:

```text
canonical product / architecture state = pushed GitHub repository
canonical execution-work state         = GitHub Issues
```

Do not mirror issue bodies or status into repository Markdown merely for canonicality. If local Markdown is selected instead, those upstream-format ticket files are the execution-work state and must be persisted wherever later workers are expected to read them.

Ticket publication happens **after** the canonical spec is persisted, so tickets can reference the canonical spec/path/commit. `TICKETS COMPLETE` means the user-approved breakdown has been published to the selected canonical tracker.

Each ticket should use upstream `to-tickets` semantics and contain only the information needed to execute the slice. In addition to the upstream behaviour, acceptance criteria, and blockers, include a concise canonical spec reference and the verification path when that prevents ambiguity. Do not copy the whole spec into every ticket.

For each fresh IMPLEMENT conversation, the coordinator should select one ticket from the current **frontier**: an open ticket whose blockers are all complete, respecting any tracker-native claim/assignment state. Seed that fresh session with the thin self-locating dispatch defined above, not a full handoff summary. The implementation session must not invent a replacement ticket or a new conversation-local slice. After tracker lifecycle state is updated, recompute the frontier from the canonical tracker.

ADRs, `CONTEXT.md`, roadmaps, and other project-management artifacts remain opt-in and require their own concrete value.

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
- Never describe repository patch applicability as verified unless it was checked against the exact canonical base, or exact identity of the verification base was proven against canonical state.
- Never hand off user-editable semantic placeholders when the local role is deterministic execution.
- Never claim a repository-backed SPEC phase is complete before its canonical persistence step succeeds.
- Never let `/implement` invent conversation-local slices for a genuine multi-session build before canonical ticket decomposition is complete.
- Never change the selected tracker backend merely because one connector or write transport failed.

## Precedence

When upstream instructions conflict with this adapter only because they assume a local coding-agent environment, this adapter wins.

For workflow semantics that are not Web/local-environment differences, upstream wins.
