# Agentic Engineering — Template Kit

A reusable, ready-to-fill toolkit extracted from *Agentic Engineering*
(ed. 2.0, the textbook). The book’s core claim is that **the task specification is
the single load-bearing artifact** of the agentic workflow — everything else (review,
verification, mutation control, failure handling) hangs off it. These files turn that
claim into fill-in-the-blank working surfaces.

## Files

| File | Source (book) | Purpose |
| --- | --- | --- |
| `spec-template.md` | Ch. 18.1 | The task specification: objective, scope, do-not-change, publication intent, criteria |
| `acceptance-criteria.md` | Ch. 18.2 / 7.4 | Turn adjectives into executable checks |
| `review-checklist.md` | Ch. 18.3 / 8.6 | Review the diff as evidence before accepting |
| `blocker-record.md` | Ch. 18.4 / 11.6 | Record a stalled route and pivot |
| `task-lifecycle-checklist.md` | Ch. 20 / 6 | End-to-end gate checklist for the five-stage loop |

## How to use

1. Copy `spec-template.md` into a per-task file and fill in EVERY field.
2. Use the acceptance-criteria semantics when writing the criteria.
3. Run the review checklist against the agent’s diff before accepting.
4. On a stall, record a blocker and pivot — the route stalls, the goal does not.
5. Track the five-stage loop with the lifecycle checklist.

## The six load-bearing principles (book, Ch. 4)

1. Own intent, architecture, and acceptance.
2. Specify before you delegate.
3. Review code as evidence.
4. Verify before you accept.
5. Control mutation and publication.
6. Treat failure as route failure.

> The agent is a capable executor and an untrustworthy authority. Accept nothing on its
> word; climb the trust ladder from skepticism to a recorded acceptance.
