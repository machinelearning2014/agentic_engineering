# Acceptance Criteria — How to Write Them

> Source: *Agentic Engineering* (ed. 2.0), Chapter 18.2 + Section 7.4.
> An acceptance criterion is valid if and only if it is falsifiable, observable, and decidable.
> **This file is a reference.** Record your task's actual criteria in `spec-template.md`
> (the `## Acceptance criteria` section) — that is the single source `ae` reads.

## The sentence pattern

Every criterion is phrased as:

> “When I run `<command>`, I observe `<result>`.”

If you cannot fill in `<command>` and `<result>`, the criterion is not finished.

## Examples

- “When I run `make test`, all tests pass and no test file was modified.”
- “When I run `git diff -- src/auth/index.ts`, the output is empty.”
- “When I run `gitleaks detect`, it reports zero findings.”
- “When I run `make lint && make build`, both exit 0.”

## Weak (adjective) → strong (executable)

| Weak | Strong |
| --- | --- |
| “It should be fast” | “p95 latency < 400 ms in the given load test” |
| “It should handle errors” | “On a 5xx the client retries with backoff and surfaces a typed error; covered by `TestRetryOn5xx`” |
| “It should be secure” | “No secret appears in logs or the diff; `gitleaks` reports zero findings” |
| “It should not break things” | “Full suite passes; `make lint` and `make build` exit 0” |

## The three tests (Chapter 7.4)

- [ ] **Falsifiable** — the criterion must be able to fail.
- [ ] **Observable** — the evidence is visible to YOU, not only to the agent.
- [ ] **Decidable** — a procedure settles pass/fail with no reasonable disagreement.
