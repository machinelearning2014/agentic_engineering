# Agentic Engineering — Spec-Driven Development Toolkit

A reusable toolkit extracted from *Agentic Engineering* (ed. 2.0). The book's core
claim: **the task specification is the single load-bearing artifact** of the agentic
workflow — review, verification, mutation control, and failure handling all hang off
it. These files turn that claim into fill-in-the-blank working surfaces you copy into
your own project.

## Get the kit

One-time setup — put the kit files **inside your project**, in the directory you'll run
the agent from. Download or clone this repo, then copy `ae`, `AGENTS.md`, and the five
templates (see **Files**) into your project:

```bash
git clone https://github.com/machinelearning2014/agentic_engineering.git
cp agentic_engineering/ae agentic_engineering/AGENTS.md \
   agentic_engineering/{spec-template,acceptance-criteria,review-checklist,blocker-record,task-lifecycle-checklist}.md \
   /path/to/your-project/
```

`./ae` is run from that project directory, and it creates `tasks/` there — so the
**Scope** paths in your spec are relative to your project.

## Quick start

From your project directory (the one containing `ae`), per task:

```bash
./ae new getuser-retry                          # scaffold tasks/getuser-retry/spec.md
#     … fill in EVERY field …
./ae verify getuser-retry                       # no blanks, criteria executable
#     … hand the spec to your agent …
./ae review getuser-retry                         # auto-detects diff facts + scaffolds review.md
#     … review the facts, check off all 8 items …
./ae accept getuser-retry --evidence "npm test green"   # record the decision
```

See `example.md` for a complete filled-in run.

## How it works

The five-stage loop — **Specify → Delegate → Review → Verify → Accept**
(or reject → re-enter):

| Stage | Who | Surface |
| --- | --- | --- |
| Specify | Human | `spec-template.md` — the spec is the load-bearing artifact |
| Delegate | Agent | `AGENTS.md` + the filled spec — the agent executes only, inside scope |
| Review | Human | `ae review` — auto-detected diff facts, then `review-checklist.md` to check off |
| Verify | Human | `acceptance-criteria.md` + `ae verify` — executable checks |
| Accept | Human | `ae accept` / `ae reject` — a recorded decision |

The **human** authors the spec and owns the four decisions (specify, delegate, review,
accept); the **agent** only executes inside the scoped loop and returns a diff +
evidence. The agent is a capable executor and an untrustworthy authority — accept
nothing on its word.

"Copy" throughout this kit means **duplicate a file**, not clone this repo — cloning
is only how you obtain the kit once; `./ae new <task>` is what instantiates a spec per
task.

## Files

| File | Source (book) | Purpose |
| --- | --- | --- |
| `spec-template.md` | Ch. 18.1 | Objective, scope, do-not-change, publication intent, criteria, output contract |
| `acceptance-criteria.md` | Ch. 18.2 / 7.4 | Turn adjectives into executable checks |
| `review-checklist.md` | Ch. 18.3 / 8.6 | Review the diff as evidence before accepting |
| `blocker-record.md` | Ch. 18.4 / 11.6 | Record a stalled route and pivot |
| `task-lifecycle-checklist.md` | Ch. 20 / 6 | End-to-end gate checklist for the loop |
| `AGENTS.md` | — | Standing instructions the agent reads before working |
| `ae` | — | Task helper: `ae new/verify/blocker/accept/reject/status` |
| `example.md` | — | A fully worked run of the kit on one small task |
| `openspec-comparison.md` | — | How this kit relates to the OpenSpec framework |

## The `ae` helper

One command per stage (plain bash, no dependencies):

| Command | What it does |
| --- | --- |
| `./ae new <task>` | Scaffold `tasks/<task>/spec.md` |
| `./ae verify [<task>]` | Lint: no blanks, criteria executable, one publication-intent checkbox |
| `./ae blocker <task>` | Scaffold `tasks/<task>/blocker.md` |
| `./ae review <task>` | Scaffold `tasks/<task>/review.md` — auto-detects changed files, scope (paths or globs), deps, test changes |
| `./ae accept <task> --evidence "…"` | Record ACCEPT (refuses unless verify passes) |
| `./ae reject <task> --reason "…"` | Record REJECT |
| `./ae status` | Show each task's verify, review, and decision state |

`verify` exits non-zero on a failing spec, so it also works as a CI gate.

## The six load-bearing principles (book, Ch. 4)

1. Own intent, architecture, and acceptance.
2. Specify before you delegate.
3. Review code as evidence.
4. Verify before you accept.
5. Control mutation and publication.
6. Treat failure as route failure.

> The agent is a capable executor and an untrustworthy authority. Accept nothing on its
> word; climb the trust ladder from skepticism to a recorded acceptance.
