# Sanitized Excerpts (Edited from Internal Docs)

This file is a **public-safe** set of excerpts from internal documentation.
Edits remove proprietary details, file paths, and any sensitive fixtures.

## Excerpt: Why “visual correctness” matters
- Rendering systems need verification because “looks right” cannot be trusted at scale.
- Proofs prevent regressions during refactors and format edge-case work.

## Excerpt: Sprint discipline
- One user-facing outcome + one proof.
- No scope additions after promotion.
- Only compound on green.

## Excerpt: Test taxonomy (conceptual)
- contracts: ergonomics / invariants
- features: behavior tests (actions mutate state)
- oracles: fidelity comparisons (visual correctness)

## Excerpt: Safety boundary
- AI assistance is valuable, but must operate within explicit constraints.
- Verification is the gate, not the model’s confidence.
