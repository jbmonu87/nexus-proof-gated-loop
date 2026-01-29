# Build Loop (AI-Assisted, Proof-Gated)

## Goal
Ship high-fidelity document workflows with AI assistance **without** sacrificing correctness.

## Roles
### Planner (Verifier/Plan Reviewer)
- Defines the smallest shippable outcome
- Writes the acceptance criteria as a spec
- Rejects scope creep

### Builder (Coder)
- Implements only what the spec requires
- Adds seams/selectors needed for verification
- Avoids large refactors unless explicitly scoped

### Verifier (Test & Fidelity)
- Runs the proof (tests / oracles)
- Blocks progress on failures
- Produces a minimal failure report when things break

## Atomic Sprint rule
Each sprint has:
1) **One user-facing outcome**
2) **One proof** (an automated check that fails first and goes green)

## Why this works
- It turns “AI speed” into *compounding progress* instead of compounding chaos.
- It makes quality legible to reviewers (and future maintainers).
- It reduces the cost of refactors because contracts stay stable.

## Definition of done
- The proof passes
- The change is explainable in one paragraph
- Any new behavior has a contract / invariant or test
