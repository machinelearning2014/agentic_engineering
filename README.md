# Agentic Engineering — Template Kit

A reusable, ready-to-fill toolkit extracted from *Agentic Engineering*
(ed. 2.0, the textbook). The book’s core claim is that **the task specification is
the single load-bearing artifact** of the agentic workflow — everything else (review,
verification, mutation control, failure handling) hangs off it. These files turn that
claim into fill-in-the-blank working surfaces.

## Who uses this, and what “copy” means

Two audiences, two jobs — the split is not optional:

| Audience | Job in the loop | Uses |
| --- | --- | --- |
| **You (the human)** | Specify, Review, Verify, Accept | Fill in the templates; make the decisions |
| **The AI coding agent** | Delegate: execute inside the scoped loop | Reads the *filled* spec + criteria; returns a diff + evidence |

The templates are **fill-in-the-blank surfaces for you**, the human. The *filled*
specification (objective, scope, do-not-change, publication intent, criteria) is the
**delegation artifact** — the thing you hand to the agent, as text, when you delegate.
The review, blocker, and lifecycle checklists are **your** decision surfaces, not files
the agent consumes. You may show them to the agent for context, but you carry the
four decisions: specification, delegation, review, acceptance.

**“Copy” = duplicate a file, not `git clone` this repo.** Concretely:

- **Reuse the kit in your project**: copy the five template files into your own repo
  (or `git clone` this repo once and keep the files you want). Adapt them to your tooling.
- **Per task**: duplicate one template into a fresh file and fill it in, e.g.
  `cp spec-template.md tasks/getuser-retry.md`.

There is no library to import and nothing for the agent to install. The agent never
touches this repository — it receives a *filled* spec as text (pasted into the agent’s
prompt or pointed at by path), then works inside the scoped loop.

## Files

| File | Source (book) | Purpose |
| --- | --- | --- |
| `spec-template.md` | Ch. 18.1 | The task specification: objective, scope, do-not-change, publication intent, criteria |
| `acceptance-criteria.md` | Ch. 18.2 / 7.4 | Turn adjectives into executable checks |
| `review-checklist.md` | Ch. 18.3 / 8.6 | Review the diff as evidence before accepting |
| `blocker-record.md` | Ch. 18.4 / 11.6 | Record a stalled route and pivot |
| `task-lifecycle-checklist.md` | Ch. 20 / 6 | End-to-end gate checklist for the five-stage loop |
| `example.md` | — | A fully worked run of the kit on one small task |

## How to use

The kit follows the five-stage loop: **Specify → Delegate → Review → Verify → Accept**
(or reject → re-enter). Each file below is one surface in that loop.

### 1. Write the specification first (`spec-template.md`)

Copy `spec-template.md` into a per-task file (duplicate the file, e.g.
`cp spec-template.md tasks/<task-name>.md` — see “Who uses this” above) and fill in
EVERY field before delegating. The filled file is what you hand to the agent.
A blank field is a gap the agent will fill with something plausible-but-wrong.

- **Objective** — one falsifiable sentence. “Make it better” fails; “`make build`
  exits 0 and the full test suite passes” works.
- **Scope** — the exact files/modules/directories that MAY change. Anything not listed
  is out of bounds by default.
- **Do not change** — invariants, public APIs, and preserved behavior. This is your
  mutation boundary.
- **Publication intent** — pick one checkbox. Default is **None** (no commit, push,
  or PR). Never let the agent assume publication.
- **Acceptance criteria** — see step 2.
- **Output contract** — what the agent must report back: change summary, evidence
  (exit codes, logs, test output), and known limitations.

### 2. Make every criterion executable (`acceptance-criteria.md`)

Each criterion must be falsifiable, observable, and decidable — phrased as:

> “When I run `<command>`, I observe `<result>`.”

If you cannot fill in `<command>` and `<result>`, the criterion is not finished.
Replace adjectives with checks: “secure” → “`gitleaks detect` reports zero findings”;
“doesn’t break things” → “full suite passes and `make lint` exits 0”. A third party
should be able to run acceptance with zero clarifying questions.

### 3. Review the diff, not the summary (`review-checklist.md`)

After the agent runs, read the **diff first**, then the context (callers, tests,
config, error handling). Hunt the six failure classes — scope creep, behavior change
under cover, silent gap-filling, error-handling regressions, security-relevant changes,
and test weakening. Separate **fact** (what the source shows) from **inference** (what
you conclude) from **claim** (what the agent says — unverified until inspected).

### 4. On a stall, record and pivot (`blocker-record.md`)

The route stalls, the goal does not. Record four fields: **goal**, **route attempted**,
**observed failure** (verbatim, not paraphrased), and **next route planned**. A retry is
only valid if it differs in kind — a different decomposition, library, technique, or
order of work. Otherwise escalate to a human.

### 5. Gate the whole loop (`task-lifecycle-checklist.md`)

Track the five stages end-to-end. Each stage has an entry and exit gate:

| Stage | Exit gate |
| --- | --- |
| Specify | Spec is falsifiable and third-party checkable |
| Delegate | Agent ran and produced artifacts + claims |
| Review | You can state what changed and what it might break |
| Verify | Every criterion has a passing, inspected check |
| Accept | A recorded accept/reject decision exists |

Rejection re-enters the loop with a written reason (spec gap / route failure /
verification failure). Remember the four points where only the human acts:
specification, delegation, review, and acceptance.

See `example.md` for a complete worked run — a filled-in spec, executable criteria,
a review that catches real failures, a blocker record, and the gated accept.

## The six load-bearing principles (book, Ch. 4)

1. Own intent, architecture, and acceptance.
2. Specify before you delegate.
3. Review code as evidence.
4. Verify before you accept.
5. Control mutation and publication.
6. Treat failure as route failure.

> The agent is a capable executor and an untrustworthy authority. Accept nothing on its
> word; climb the trust ladder from skepticism to a recorded acceptance.
