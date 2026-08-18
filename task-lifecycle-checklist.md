# Agentic Engineering Lifecycle Checklist

> Source: *Agentic Engineering* (ed. 2.0), Chapter 6 + 20.
> The core loop: Specify → Delegate → Review → Verify → Accept (or reject → re-enter).

## Before delegating (Specify)

- [ ] The outcome is one falsifiable sentence.
- [ ] Mutation intent names the scope and the invariants to preserve.
- [ ] Publication intent is stated (default: none).
- [ ] Every acceptance criterion is an executable check (falsifiable, observable, decidable).
- [ ] A third party could check acceptance without a single clarifying question.

## Delegate

- [ ] Scope is explicit: which files/modules are in scope, which tools, what NOT to do.
- [ ] The agent may run autonomously within the scoped loop (plan, write, test, iterate).
- [ ] The human holds the boundary and the exit conditions.

## Review

- [ ] I read the diff, not the summary.
- [ ] I ran the review checklist (review-checklist.md), all items checked.
- [ ] I can state from the source what changed and what it might break.

## Verify

- [ ] Every acceptance criterion has a passing, inspected check.
- [ ] Checks are proportional to the surface the change touches.
- [ ] Evidence is executable (log, exit code, proof), not the agent’s word.

## Accept or reject (recorded decision)

- [ ] Accept: the change is done and may proceed to whatever publication was authorized.
- [ ] Reject: the loop re-enters with a written reason (spec gap / route failure / verification failure).
- [ ] The decision is recorded — what was accepted, and on what evidence.

## Entry / exit gates (Chapter 6.7)

| Stage | Entry | Exit |
| --- | --- | --- |
| Specify | A goal exists | Spec is falsifiable and third-party checkable |
| Delegate | Spec is complete | Agent ran and produced artifacts + claims |
| Review | Artifacts and claims exist | You can state what changed and what it might break |
| Verify | Review complete | Every criterion has a passing, inspected check |
| Accept | Verification complete | A recorded accept/reject decision exists |

## The four human intervention points (Chapter 6.8)

1. **Specification** — only the human knows what and why.
2. **Delegation** — only the human can scope the handoff and grant authority.
3. **Review** — only the human carries the accountability.
4. **Acceptance** — only the human decides to ship.
