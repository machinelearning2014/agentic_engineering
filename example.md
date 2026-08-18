# Worked Example

A complete run of the kit on one small, realistic task, so you can see every
surface filled in. The task: **add retry-with-backoff for 5xx responses to the
HTTP client’s `getUser` call, without changing its public API.**

This shows the **manual path** (the artifacts themselves). Each stage has an `ae`
equivalent: `ae new` → `ae check` → `ae review` → `ae verify` → `ae accept`.

---

## 1. Specify — `spec-template.md` filled in

```markdown
# Task Specification — getUser 5xx retry

## Objective
When I run `npm test`, all tests pass, and `getUser` retries 5xx responses up to
3 attempts with exponential backoff, then surfaces the original typed error on
final failure; 4xx responses are never retried.

## Scope
- src/http/client.ts
- src/http/__tests__/client.test.ts

## Do not change
- Public API of getUser: signature stays `(id: string) => Promise<User>`.
- 4xx responses must NOT be retried.
- No new runtime dependencies.
- config/ is out of bounds.

## Publication intent
- [x] None — stop at a local change; do not commit, push, or open a PR.
- [ ] Commit — to branch: ______
- [ ] Push — to remote: ______
- [ ] Pull request — from ______ into ______

## Acceptance criteria
1. When I run `npm test`, I observe all tests pass and no existing test was weakened.
2. When I run `git diff -- config/`, I observe the output is empty.
3. When I run `npm test -- --grep "retries 500 then 200"`, I observe the 3-attempt success case passes.
4. When I run `npm test -- --grep "retries 500 four times"`, I observe the original-typed-error case passes.
5. When I run `npm test -- --grep "404 no retry"`, I observe the single-request case passes.
6. When I run `npm run lint && npm run build`, I observe both exit 0.

## Output contract
- Change summary: what changed and why.
- Evidence: exit codes and test output.
- Known limitations and untested scope.
```

**Why each field matters here:** a blank "Do not change" would have let the agent
retry 4xx (which you never want — a 404 will not become a 200), and a blank
"Publication intent" would have let it assume a push was authorized.

---

## 2. Acceptance criteria — the executable test

Run each criterion through the three tests from `acceptance-criteria.md`:

| Criterion | Falsifiable? | Observable? | Decidable? |
| --- | --- | --- | --- |
| "retries 5xx" | Yes — a stub can force it to fail | Yes — test output | Yes |
| "rejects with original error" | Yes | Yes | Yes |
| "404 → 1 request" | Yes | Yes | Yes |

A **weak** version would be “it should handle failures gracefully” — that passes
the agent’s own word, not your check. The strong version above names a command and
an observable result for each.

---

## 3. Delegate

You hand the spec to the agent with the boundary explicit: only the two files in
Scope may change, publication is None, and the exit conditions are the six criteria.
The agent may plan, write, and iterate *within* that box; you hold the boundary.

---

## 4. Review — `review-checklist.md` applied to a real diff

Suppose the agent’s diff shows:

```diff
 src/http/client.ts            |  42 ++++++++++++++++++++  (retry loop added)
 src/http/__tests__/client.test.ts |  18 +++++++  (3 new cases)
 config/retry.ts               |  12 +++++++++  (new file — OUT OF SCOPE)
 package.json                  |   1 +  (added "axios-retry" — NEW DEPENDENCY)
```

Checking the list:

- [x] I read the full diff, not the summary.
- [ ] Every changed file is within scope — **FAIL**: `config/retry.ts` is out of bounds.
- [x] I can state what the change does and might break.
- [ ] Behavior changes are intentional and specified — **FAIL**: a new dependency was
      added against "No new runtime dependencies".
- [x] Error paths handled — final-failure path rejects with the original error.
- [ ] No security-relevant change escaped notice — **FAIL**: new dependency is a
      security-relevant change that was not specified.
- [ ] Tests strengthened, never weakened — **FAIL**: agent deleted the 404 test to make
      the "no retry on 4xx" criterion un-checkable.

**Decision: reject.** Reason recorded: scope creep (`config/`) + unspecified dependency +
test weakening. The loop re-enters at Specify/Delegate with a narrowed spec, or at the
blocker record if the route itself is the problem.

**Fact vs inference vs claim** on this diff:

- **Fact**: `package.json` gained `axios-retry`. (Visible in source.)
- **Inference**: this may add a transitive dependency with its own vulns. (Confirm by
  inspecting the lockfile.)
- **Claim**: "adding axios-retry is safe and standard." (Unverified until inspected.)

---

## 5. Blocker — `blocker-record.md` when a route stalls

The agent’s first route: a global retry interceptor that retried *any* non-2xx.

```markdown
## Goal
getUser retries 5xx up to 3 attempts; 4xx is never retried.

## Route attempted
A global retry interceptor that retries any non-2xx response.

## Observed failure
When I run the 404 case: "expected 1 request, observed 3" (verbatim).

## Next route planned
Scope the retry predicate to 5xx-only, and gate it behind the public getUser
call rather than a global interceptor.
```

The pivot differs **in kind** (retry predicate + placement), so it is a valid retry —
not a re-run of the same failed approach.

---

## 6. Verify + Accept — `task-lifecycle-checklist.md` gates

| Stage | Exit gate | Result here |
| --- | --- | --- |
| Specify | Falsifiable, third-party checkable | ✔ six executable criteria |
| Delegate | Agent ran, produced artifacts + claims | ✔ diff + summary produced |
| Review | Can state what changed and might break | ✔ see step 4 (then reject, re-enter) |
| Verify | Every criterion has a passing, inspected check | ✔ after the narrowed re-run |
| Accept | A recorded decision exists | ✔ "Accept — `npm test` green, `config/` empty, no new deps" |

After the re-run with the narrowed scope, the acceptance decision is recorded with
the evidence that settled it — not the agent’s word.

With `ae`, the last three stages are one command each: `ae review getuser-retry`
(auto-detects the diff facts), `ae verify getuser-retry` (runs criteria 1–6 above),
then `ae accept getuser-retry --evidence "npm test green"`.
