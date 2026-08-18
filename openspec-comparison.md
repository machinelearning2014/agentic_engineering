# OpenSpec vs. the Agentic Engineering Spec-Driven Development Toolkit

## TL;DR

They solve the same problem — *make an AI coding agent do predictable work* — with
opposite centers of gravity:

- **OpenSpec** is a **tool**: an installable CLI + folder convention that turns
  spec-driven development into agent-native slash commands (`/opsx:new`, `/opsx:ff`,
  `/opsx:apply`, `/opsx:archive`). The **agent drafts the spec**; the human reviews
  intent before code.
- **The Agentic Engineering toolkit** is a **method**: fill-in-the-blank
  Markdown documents. The **human authors the spec**; the agent only executes inside
  a boundary. There is no software.

They are complementary, not competing: OpenSpec supplies the orchestration and
knowledge base; the AE kit supplies the *content standards* (executable acceptance
criteria, mutation/publication control, diff review, failure handling) that OpenSpec
leaves implicit.

---

## 1. What OpenSpec is

OpenSpec ([Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec)) is a
lightweight **spec-driven development (SDD) framework** for AI coding assistants.

- **Delivery**: an npm package — `npm i -g @fission-ai/openspec` — plus an `openspec/`
  directory in your repo and an `AGENTS.md` instruction file. No API keys; an optional
  MCP server adds a dashboard and approval workflow.
- **Native integration**: 20+ tools (Claude Code, Cursor, Codex, GitHub Copilot,
  Windsurf, Cline, etc.) with built-in slash commands.
- **Loop**: Proposal → Planning → Implementation → Archive.
  - `/opsx:new <name>` — create a change folder.
  - `/opsx:ff` — generate proposal + specs + design + tasks ("fast-forward").
  - `/opsx:apply` — implement the tasks.
  - `/opsx:archive` — consolidate into permanent specs.
- **Artifacts** per change:
  - `proposal.md` — why, what's changing, scope, success criteria.
  - `specs/` — Requirements ("The system SHALL…") + Scenarios ("WHEN/THEN"), expressed
    as **spec deltas** (ADDED / MODIFIED / REMOVED) so reviewers see the *change in
    requirements*, not just the code.
  - `design.md` — technical approach and trade-offs.
  - `tasks.md` — ordered implementation checklist.
- **Knowledge accumulation**: archiving folds a change into `openspec/specs/`, so the
  system's requirements persist beyond the chat session and future changes build on
  them. Optional **Stores** extend this across repos and teams.
- **Philosophy** (verbatim): *fluid not rigid, iterative not waterfall, easy not
  complex, built for brownfield not just greenfield, scalable from personal projects
  to enterprises.*

**Who does what in OpenSpec:** the **agent writes** the proposal, specs, design, and
tasks; the **human reviews** them *before* `/opsx:apply`, then reviews the
implementation, then archives. The human's lever is "review intent, not just code."

---

## 2. What the Agentic Engineering toolkit is

Five fill-in-the-blank Markdown documents distilled from the *Agentic Engineering*
textbook (ed. 2.0), plus a worked example:

| File | Job |
| --- | --- |
| `spec-template.md` | Objective, Scope, Do-not-change, Publication intent, Acceptance criteria, Output contract |
| `acceptance-criteria.md` | Turn adjectives into falsifiable/observable/decidable checks |
| `review-checklist.md` | Review the diff as evidence; six failure classes; fact/inference/claim |
| `blocker-record.md` | Record a stalled route and pivot (route fails, goal does not) |
| `task-lifecycle-checklist.md` | Gate the five-stage loop Specify → Delegate → Review → Verify → Accept |

- **Delivery**: plain Markdown. No CLI, no install, no slash commands, no runtime.
  You copy a template into a per-task file (`cp spec-template.md tasks/foo.md`) and
  hand the *filled* file to the agent as text.
- **Loop**: Specify → Delegate → Review → Verify → Accept (or reject → re-enter),
  each stage with entry/exit gates.
- **Philosophy**: the spec is "the single load-bearing artifact"; the agent is "a
  capable executor and an untrustworthy authority"; "accept nothing on its word."

**Who does what in the AE kit:** the **human authors** every field of the spec
("only the human knows what and why"), and owns four decisions — specification,
delegation, review, acceptance. The **agent** only executes inside the scoped loop
and returns a diff + evidence.

---

## 3. Side-by-side

| Dimension | OpenSpec | AE toolkit |
| --- | --- | --- |
| Category | Tool / framework (CLI + conventions) | Method / documents |
| Installable? | Yes (`npm i -g @fission-ai/openspec`) | No — copy Markdown |
| Agent-native? | Yes — slash commands, `AGENTS.md` | No — paste/point at a filled file |
| Who drafts the spec | **Agent** (you review) | **Human** (you write) |
| Spec content | Requirements + Scenarios (SHALL / WHEN-THEN) | Objective, scope, do-not-change, publication intent, criteria |
| Acceptance criteria | "Success criteria" in proposal (unstructured) | **Executable checks** — "When I run `<cmd>`, I observe `<result>`", must be falsifiable/observable/decidable |
| Mutation control | Scope in proposal; relies on the agent's git behavior | Explicit **Do-not-change** list + **Publication intent** checkbox (default: none) |
| Review | "Review intent, not just code"; spec deltas | **Diff-first checklist** + six failure classes + fact/inference/claim |
| Failure handling | Fluid iteration; update artifacts | **Route vs. goal**; blocker record; "materially different retries"; escalate |
| Knowledge base | Yes — archive consolidates into `openspec/specs/` | No — per-task files only |
| Team / cross-repo | Yes — Stores, shared specs, dashboard, Kanban | No (single human + agent per task) |
| Brownfield | Built for it | Neutral (assumes you bring your own repo) |
| Rigor gates | Deliberately "fluid, no rigid gates" | Explicit entry/exit gates per stage |
| Reject path | Implicit (edit artifacts, re-apply) | Explicit (reject → re-enter with written reason) |

---

## 4. The one difference that matters

**Who writes the specification.**

- OpenSpec assumes the agent is competent enough to *propose* the change — the
  human's job is to catch a bad proposal before code exists. It optimizes for
  **throughput and knowledge accumulation**.
- The AE kit assumes the spec must come from the human, because *every gap in the
  spec is a gap the agent silently fills with something plausible-but-wrong*. It
  optimizes for **control and verifiability**.

These aren't mutually exclusive. OpenSpec's own docs say to "always review the
proposal and specs before implementing" — which is exactly where the AE kit's
standards would plug in.

---

## 5. Where each is stronger

**OpenSpec is stronger at:**
- Orchestration (slash commands, native tool integration, MCP/dashboard).
- Persistent requirements ("specs as knowledge base", archive).
- Team scale and cross-repo planning (Stores).
- Reducing review surface (concise ~250-line specs, intent-first deltas).

**The AE kit is stronger at:**
- Making acceptance criteria *executable* (falsifiable / observable / decidable).
- Controlling mutation and **publication** (explicit do-not-change + publication
  intent; OpenSpec has no equivalent of a "do not commit/push" field).
- Structured code review (diff-first, failure-class hunting, fact vs. inference vs. claim).
- Failure discipline (route vs. goal, blocker record, materially different retries).

---

## 6. How to combine them

They slot together cleanly — OpenSpec is the *conveyor belt*, the AE kit is the
*quality standard for what travels on it*:

1. **At proposal time**: when the agent drafts `proposal.md`, require it to fill the
   AE spec fields — Objective, Scope, **Do-not-change**, **Publication intent**,
   Acceptance criteria, Output contract — as sections of the proposal.
2. **At planning time**: rewrite OpenSpec's "success criteria" as AE-style executable
   criteria ("When I run `<cmd>`, I observe `<result>`") inside `specs/`.
3. **At review time** (before `/opsx:apply` and again before `/opsx:archive`): run
   `review-checklist.md` on the spec delta *and* on the code diff — read the diff,
   hunt the six failure classes, separate fact/inference/claim.
4. **At failure time**: if a route stalls during `/opsx:apply`, write a
   `blocker-record.md` and require the retry to differ in kind before re-applying.
5. **At acceptance time**: gate the archive step on `task-lifecycle-checklist.md` —
   every criterion has a passing, inspected check, and the decision is recorded.

**Net**: use OpenSpec when you want tooling, agent-native flow, and a growing spec
base across a team. Use the AE kit's content standards regardless — whether you paste
them into OpenSpec proposals or into any other agent. The AE kit is the smaller, more
portable idea; OpenSpec is the larger, more automated idea.
