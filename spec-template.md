# Task Specification Template

> Source: *Agentic Engineering* (ed. 2.0), Chapter 18.1.
> The specification is where quality enters the system: every gap here is a gap the
> agent will silently fill with something plausible-but-wrong. Fill every field
> before delegating; leave none blank.

## Objective

_One falsifiable sentence: what must be true when the work is done._

<Write it here — a concrete, measurable outcome, not an adjective.>

## Scope

_Files / modules / directories that MAY be changed._

- <path or module>

## Do not change

_Invariants, public APIs, behavior to preserve, files out of bounds._

- <invariant to preserve, e.g. public API of X, external behavior of Y>

## Publication intent

_Choose one. Default is NONE._

- [ ] None — stop at a local change; do not commit, push, or open a PR.
- [ ] Commit — to branch: ______
- [ ] Push — to remote: ______
- [ ] Pull request — from ______ into ______

## Acceptance criteria

_Each criterion must be an executable check: a command whose output settles pass/fail._
_Falsifiable, observable, decidable (see acceptance-criteria.md)._

1. <Executable check: command + expected result.>
2. <Executable check.>
3. <Executable check.>

## Output contract

_What the agent must report back after the work._

- <Change summary: what changed and why.>
- <Evidence: exit codes, logs, test output.>
- <Known limitations and untested scope.>
