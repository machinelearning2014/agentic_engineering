# Review Checklist

> Source: *Agentic Engineering* (ed. 2.0), Chapter 8.6 / 18.3.
> Review the diff as evidence — the agent’s summary is a pointer, not a result.
> Run this list before ANY acceptance decision.

Read the **diff first**, then the **context** (callers, tests, configuration, error handling).

## Checklist

- [ ] 1. I read the full diff, not the summary.
- [ ] 2. Every changed file is within the authorized mutation scope.
- [ ] 3. I can state, from the source, what the change does and what it might break.
- [ ] 4. Behavior changes, if any, are intentional and specified.
- [ ] 5. Error paths and edge cases are handled, not just the happy path.
- [ ] 6. No security-relevant change (dependency, permission, secret, validation) escaped notice.
- [ ] 7. Tests were strengthened or correctly updated, never weakened to pass.
- [ ] 8. My material inferences were confirmed against the source, not assumed.

## Failure classes to hunt (in order of danger)

- **Scope creep** — changes outside the requested surface.
- **Behavior change under cover** — a “refactor” that alters a return value, default, or edge case.
- **Silent gap-filling** — a requirement the spec never stated, implemented without flagging.
- **Error-handling regressions** — swallowed exceptions, removed retries, changed timeouts, weakened validation.
- **Security-relevant changes** — new deps, changed permissions, added network calls, secrets, weakened input validation.
- **Test weakening** — tests deleted, skipped, or loosened rather than fixed.

## Facts vs. inferences

- **Fact**: what the source directly shows (“the function now returns `Option`”).
- **Inference**: what you conclude, with a risk of error (“this will break the caller”) — confirm against source.
- **Neither**: the agent’s claim (“this is safe”) — unverified until you inspect.
