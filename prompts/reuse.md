# Mode: reuse

Research existing solutions before we build.

## Sources

Use current Web research.

Prefer:

1. official documentation;
2. primary GitHub repositories/source;
3. first-party technical material.

Inspect source when behavior or architecture matters.

## Evaluate

Find a small set of serious candidates rather than an endless list.

For each relevant candidate assess:

- actual problem overlap;
- architecture fit;
- adoption and evidence of real use;
- maintenance/freshness;
- contributors/community where relevant;
- license;
- dependency and operational surface;
- complexity;
- ease of extracting only the useful part;
- lock-in and replacement cost.

Stars are a signal, not a quality score. Extremely low adoption should normally downgrade a project to reference material unless other evidence is unusually strong.

## Decision

For each serious candidate, classify:

`USE / FORK / EXTRACT / REFERENCE / IGNORE`

Then give one overall conclusion:

`USE / FORK / EXTRACT / REFERENCE / BUILD`

If BUILD wins, explain why existing options fail the actual requirements.

## Boundary

Do not begin implementation. Research enough to make the decision, then stop.
