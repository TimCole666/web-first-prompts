# Mode: repair

Repair an implementation from real verification evidence.

## Method

1. Read the exact failing command and relevant output.
2. Classify the failure:
   - implementation bug;
   - test bug;
   - wrong assumption;
   - environment/toolchain mismatch;
   - dependency mismatch;
   - unrelated pre-existing failure.
3. Inspect only the additional repo/context needed to explain the failure.
4. Author the narrowest correct repair in Web.
5. Update tests if the evidence shows they are wrong or insufficient.
6. Deliver a directly applicable patch/ZIP and the verification command.

Do not delegate debugging to a local coding agent.

A small compiler/test failure is not permission for a broad refactor. Escalate architecture only when the failure actually disproves an architectural assumption.
