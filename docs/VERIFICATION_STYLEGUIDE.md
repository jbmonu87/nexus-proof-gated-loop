# Verification Style Guide (How “Correct” is enforced)

## Principle
If a behavior matters, it must be checkable.

## What we verify
- User-visible behavior (clicks mutate state)
- UX invariants (layout/interaction rules never regress)
- Visual fidelity (render output matches expectation)

## How we reduce flakiness
- Prefer deterministic selectors and stable seams over brittle DOM heuristics
- Avoid timing-based waits; prefer explicit readiness signals where possible
- Keep tests small and scoped to one behavior

## Evidence expectations
For any meaningful fix:
- One failing artifact (screenshot/error)
- The smallest plausible change
- One passing artifact after (screenshot/passing test)
