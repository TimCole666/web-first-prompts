# Launcher examples

Each fresh conversation points directly at one raw prompt file. No prompt installation or mode router is required.

## Main

```text
Follow this prompt:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/prompts/main.md

Goal: continue development of ...
Canonical state: https://github.com/ORG/REPO/tree/main
```

## Independent research

```text
Follow this prompt:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/prompts/research.md

Goal: determine ...
Relevant context: ...
Expected return: evidence-backed answer and project consequence
```

## Reuse research

```text
Follow this prompt:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/prompts/reuse.md

Goal: decide whether we should build X
Relevant context: we need ...
Expected return: USE / FORK / EXTRACT / REFERENCE / BUILD
```

## Fresh review

```text
Follow this prompt:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/prompts/review.md

Goal: independently review feature X
Canonical state: https://github.com/ORG/REPO/tree/feature-x
Fixed decisions: ...
Relevant context: acceptance criteria are ...
```

## Repair

```text
Follow this prompt:
https://raw.githubusercontent.com/TimCole666/web-first-prompts/main/prompts/repair.md

Canonical state: https://github.com/ORG/REPO/tree/feature-x
Task: repair this real failure

Command:
npm test

Output:
...
```
