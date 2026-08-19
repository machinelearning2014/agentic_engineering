# Agentic Engineering Lifecycle Checklist

> Source: *Agentic Engineering* (ed. 2.0), Chapter 6 + 20.
> The core loop: Specify → Delegate → Review → Verify → Accept (or reject → re-enter).
> Run the five phases of the working checklist (Chapter 20), gate each stage
> (Chapter 6.7), and hold the three human intervention points (Chapter 6.8).

## 1. Before delegating (Chapter 20.1)

- [ ] The outcome is one falsifiable sentence.
- [ ] Mutation intent names the scope and the invariants to preserve.
- [ ] Publication intent is stated (default: none).
- [ ] Acceptance criteria are executable checks.
- [ ] Each criterion is falsifiable, observable, and decidable.
- [ ] The specification could be checked by a third party without questions.

## 2. While the agent works (Chapter 20.2)

- [ ] The agent is working within the delegated scope.
- [ ] The agent reads source before editing.
- [ ] High-consequence actions are gated on human authorization.
- [ ] Failures are being classified, not just re-prompted.

## 3. Before accepting (Chapter 20.3)

- [ ] I read the full diff, not the summary.
- [ ] The review checklist (review-checklist.md) is complete.
- [ ] Every acceptance criterion has a passing, inspected check.
- [ ] Security-relevant changes were reviewed.
- [ ] Tests were not weakened.

## 4. Before publishing (Chapter 20.4)

- [ ] Publication is explicitly authorized for this action.
- [ ] The diff was inspected after the final mutation.
- [ ] Secrets were scanned and the scan is clean.
- [ ] The change is on the intended branch, not a shared mainline by accident.

## 5. On failure (Chapter 20.5)

- [ ] The failure is classified (spec / verification / route / capability / goal).
- [ ] The next attempt differs materially from the last.
- [ ] The blocker is recorded (goal, route, failure, next route).
- [ ] After the bounded attempts, escalate rather than loop.

## Entry / exit gates (Chapter 6.7)

| Stage | Entry | Exit |
| --- | --- | --- |
| Specify | A goal exists | Spec is falsifiable and third-party checkable |
| Delegate | Spec is complete | Agent ran and produced artifacts + claims |
| Review | Artifacts and claims exist | You can state what changed and what it might break |
| Verify | Review complete | Every criterion has a passing, inspected check |
| Accept | Verification complete | A recorded accept/reject decision exists |

## The three human intervention points (Chapter 6.8)

1. **Specification** — only the human knows what and why.
2. **Review** — only the human carries the accountability that makes scrutiny honest.
3. **Acceptance** — only the human decides to ship.

Everything between those points — planning, writing, testing, iterating, reporting —
is the agent's domain.
