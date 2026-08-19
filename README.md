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
./ae check getuser-retry                        # lint: no blanks, criteria executable
#     … hand the spec to your agent …
./ae review getuser-retry                       # auto-detects diff facts + scaffolds review.md
#     … review the facts, check off all 8 items …
./ae verify getuser-retry                       # run the acceptance-criteria commands
./ae accept getuser-retry --evidence "npm test green"   # record the decision
```

See `example.md` for a complete filled-in run.

## How it works

The five-stage loop — **Specify → Delegate → Review → Verify → Accept**
(or reject → re-enter) — gated at every stage by `task-lifecycle-checklist.md`, the
master map. Each stage has one `ae` command and one working surface:

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1160 540" width="100%" role="img" aria-label="Specify, Delegate, Review, Verify, Accept loop with ae commands, files, reject arc, and blocker branch">
  <defs>
    <marker id="arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="8" markerHeight="8" orient="auto">
      <path d="M0,0 L10,5 L0,10 z" fill="#64748b"/>
    </marker>
  </defs>

  <!-- master gate map -->
  <rect x="40" y="16" width="1080" height="44" rx="8" fill="#0f172a"/>
  <text x="580" y="44" text-anchor="middle" font-family="system-ui, sans-serif" font-size="15" font-weight="600" fill="#f8fafc">task-lifecycle-checklist.md — the master gate map (gates every stage)</text>

  <!-- 1 · Specify -->
  <rect x="40" y="88" width="200" height="180" rx="10" fill="#eff6ff" stroke="#2563eb" stroke-width="1.5"/>
  <text x="140" y="120" text-anchor="middle" font-family="system-ui, sans-serif" font-size="16" font-weight="700" fill="#1e3a8a">1 · Specify</text>
  <text x="140" y="146" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">Human</text>
  <rect x="85" y="164" width="110" height="24" rx="12" fill="#00000012"/>
  <text x="140" y="181" text-anchor="middle" font-family="ui-monospace, monospace" font-size="12" fill="#334155">ae new · ae check</text>
  <text x="140" y="210" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">spec-template.md</text>
  <text x="140" y="227" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">+ acceptance-criteria.md</text>

  <!-- 2 · Delegate -->
  <rect x="256" y="88" width="200" height="180" rx="10" fill="#f5f3ff" stroke="#7c3aed" stroke-width="1.5"/>
  <text x="356" y="120" text-anchor="middle" font-family="system-ui, sans-serif" font-size="16" font-weight="700" fill="#4c1d95">2 · Delegate</text>
  <text x="356" y="146" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">Human → Agent</text>
  <rect x="316" y="164" width="80" height="24" rx="12" fill="#00000012"/>
  <text x="356" y="181" text-anchor="middle" font-family="ui-monospace, monospace" font-size="12" fill="#334155">hand off</text>
  <text x="356" y="210" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">AGENTS.md + filled spec</text>

  <!-- 3 · Review -->
  <rect x="472" y="88" width="200" height="180" rx="10" fill="#eff6ff" stroke="#2563eb" stroke-width="1.5"/>
  <text x="572" y="120" text-anchor="middle" font-family="system-ui, sans-serif" font-size="16" font-weight="700" fill="#1e3a8a">3 · Review</text>
  <text x="572" y="146" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">Human</text>
  <rect x="532" y="164" width="80" height="24" rx="12" fill="#00000012"/>
  <text x="572" y="181" text-anchor="middle" font-family="ui-monospace, monospace" font-size="12" fill="#334155">ae review</text>
  <text x="572" y="210" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">review-checklist.md</text>

  <!-- 4 · Verify -->
  <rect x="688" y="88" width="200" height="180" rx="10" fill="#ecfdf5" stroke="#059669" stroke-width="1.5"/>
  <text x="788" y="120" text-anchor="middle" font-family="system-ui, sans-serif" font-size="16" font-weight="700" fill="#064e3b">4 · Verify</text>
  <text x="788" y="146" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">Human + Agent</text>
  <rect x="748" y="164" width="80" height="24" rx="12" fill="#00000012"/>
  <text x="788" y="181" text-anchor="middle" font-family="ui-monospace, monospace" font-size="12" fill="#334155">ae verify</text>
  <text x="788" y="210" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">run the acceptance criteria</text>

  <!-- 5 · Accept -->
  <rect x="904" y="88" width="200" height="180" rx="10" fill="#f0fdf4" stroke="#16a34a" stroke-width="1.5"/>
  <text x="1004" y="120" text-anchor="middle" font-family="system-ui, sans-serif" font-size="16" font-weight="700" fill="#14532d">5 · Accept</text>
  <text x="1004" y="146" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">Human</text>
  <rect x="922" y="164" width="164" height="24" rx="12" fill="#00000012"/>
  <text x="1004" y="181" text-anchor="middle" font-family="ui-monospace, monospace" font-size="12" fill="#334155">ae accept · ae reject</text>
  <text x="1004" y="210" text-anchor="middle" font-family="ui-monospace, monospace" font-size="11" fill="#475569">recorded decision</text>

  <!-- forward flow -->
  <line x1="240" y1="178" x2="254" y2="178" stroke="#64748b" stroke-width="1.5" marker-end="url(#arr)"/>
  <line x1="456" y1="178" x2="470" y2="178" stroke="#64748b" stroke-width="1.5" marker-end="url(#arr)"/>
  <line x1="672" y1="178" x2="686" y2="178" stroke="#64748b" stroke-width="1.5" marker-end="url(#arr)"/>
  <line x1="888" y1="178" x2="902" y2="178" stroke="#64748b" stroke-width="1.5" marker-end="url(#arr)"/>

  <!-- blocker branch -->
  <rect x="472" y="330" width="216" height="60" rx="10" fill="#fffbeb" stroke="#d97706" stroke-width="1.5"/>
  <text x="580" y="352" text-anchor="middle" font-family="ui-monospace, monospace" font-size="13" font-weight="700" fill="#78350f">blocker-record.md</text>
  <text x="580" y="372" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#92400e">record &amp; pivot</text>
  <line x1="572" y1="268" x2="572" y2="322" stroke="#92400e" stroke-width="1.5" marker-end="url(#arr)"/>
  <text x="634" y="300" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#92400e">on stall</text>
  <polyline points="472,360 356,360 356,272" fill="none" stroke="#92400e" stroke-width="1.5" marker-end="url(#arr)"/>
  <text x="420" y="350" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#92400e">pivot → back to Delegate</text>

  <!-- reject arc -->
  <path d="M 1004 268 C 1004 470 140 470 140 276" fill="none" stroke="#64748b" stroke-width="1.5" marker-end="url(#arr)"/>
  <text x="572" y="486" text-anchor="middle" font-family="system-ui, sans-serif" font-size="12" fill="#475569">reject → re-enter the loop (spec gap / route failure / verification failure)</text>
</svg>

The **human** authors the spec and owns three irreplaceable decisions — specification,
review, acceptance. The **agent** executes inside the scoped loop and returns a diff +
evidence; verification is run by both, independently. The agent is a capable executor
and an untrustworthy authority — accept nothing on its word.

The five files fit the loop as one **input** (`spec-template.md`), one **how-to**
(`acceptance-criteria.md` — teaches the spec's criteria), one **inspection**
(`review-checklist.md`), one **exception path** (`blocker-record.md` — on a stalled
route), and one **master map** (`task-lifecycle-checklist.md`).

"Copy" throughout this kit means **duplicate a file**, not clone this repo — cloning
is only how you obtain the kit once; `./ae new <task>` is what instantiates a spec per
task.

**Two paths.** The kit works with or without `ae`:

1. **Manual** — copy the templates, fill them in, and run the checklists by hand. The
   Markdown files are the whole toolkit; nothing requires the script.
2. **Semi-automatic** — use `ae` to scaffold, lint, review, verify, and record. It
   discovers the mechanical facts (diff, scope, deps, criteria exit codes); the
   decisions stay yours.

## Files

| File | Source (book) | Purpose |
| --- | --- | --- |
| `spec-template.md` | Ch. 18.1 | Objective, scope, do-not-change, publication intent, criteria, output contract |
| `acceptance-criteria.md` | Ch. 18.2 / 7.4 | Turn adjectives into executable checks |
| `review-checklist.md` | Ch. 18.3 / 8.6 | Review the diff as evidence before accepting |
| `blocker-record.md` | Ch. 18.4 / 11.6 | Record a stalled route and pivot |
| `task-lifecycle-checklist.md` | Ch. 20 / 6 | End-to-end gate checklist for the loop |
| `AGENTS.md` | — | Standing instructions the agent reads before working |
| `ae` | — | Task helper: `ae new/check/review/verify/blocker/accept/reject/status` |
| `example.md` | — | A fully worked run of the kit on one small task |
| `openspec-comparison.md` | — | How this kit relates to the OpenSpec framework |

## The `ae` helper

One command per stage (plain bash, no dependencies):

| Command | What it does |
| --- | --- |
| `./ae new <task>` | Scaffold `tasks/<task>/spec.md` |
| `./ae check [<task>]` | Lint the spec: no blanks, criteria executable, one publication-intent checkbox |
| `./ae blocker <task>` | Scaffold `tasks/<task>/blocker.md` |
| `./ae review <task>` | Scaffold `tasks/<task>/review.md` — auto-detects changed files, scope (paths or globs), deps, test changes |
| `./ae verify [<task>]` | Run the acceptance-criteria commands; records `tasks/<task>/verify.md` |
| `./ae accept <task> --evidence "…"` | Record ACCEPT (refuses unless check + verify pass) |
| `./ae reject <task> --reason "…"` | Record REJECT |
| `./ae status` | Show each task's check, review, verify, and decision state |

`check` and `verify` exit non-zero on failure, so they work as CI gates.

## The six load-bearing principles (book, Ch. 4)

1. Own intent, architecture, and acceptance.
2. Specify before you delegate.
3. Review code as evidence.
4. Verify before you accept.
5. Control mutation and publication.
6. Treat failure as route failure.

> The agent is a capable executor and an untrustworthy authority. Accept nothing on its
> word; climb the trust ladder from skepticism to a recorded acceptance.
