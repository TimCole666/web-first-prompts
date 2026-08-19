# Launcher examples

Replace the prompt-library URL with your published repository URL.

## Main

```text
Use this prompt library: https://github.com/YOUR_NAME/web-first-prompts
Mode: main

Goal: continue development of ...
Canonical state: https://github.com/ORG/REPO/tree/main
```

## Reuse research

```text
Use this prompt library: https://github.com/YOUR_NAME/web-first-prompts
Mode: reuse

Goal: decide whether we should build X
Relevant context: we need ...
Expected return: reuse/build decision
```

## Fresh review

```text
Use this prompt library: https://github.com/YOUR_NAME/web-first-prompts
Mode: review

Goal: independently review feature X
Canonical state: https://github.com/ORG/REPO/tree/feature-x
Fixed decisions: ...
Relevant context: acceptance criteria are ...
```

## Repair

```text
Use this prompt library: https://github.com/YOUR_NAME/web-first-prompts
Mode: repair

Canonical state: https://github.com/ORG/REPO/tree/feature-x
Task: repair this real failure

Command:
npm test

Output:
...
```
