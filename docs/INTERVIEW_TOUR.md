# Interview Tour (5–10 minutes)

This is the guided path I’d recommend for reviewing this repository quickly.

## 1) The method: how I build
- Read: `docs/BUILD_LOOP.md`
What to notice:
- Clear roles (Planner / Builder / Verifier)
- Small atomic outcomes
- Progress gated on proofs, not confidence

## 2) The product constraints: what must be true
- Read: `docs/VERIFICATION_STYLEGUIDE.md`
What to notice:
- Deterministic tests
- Stable selectors / seams
- Oracles for visual fidelity

## 3) The architecture framing (public-safe)
- Read: `docs/ARCHITECTURE_AT_A_GLANCE.md`
What to notice:
- “Visual correctness” as the core moat
- Contracts vs implementation details

## 4) The receipts (evidence)
- Browse: `evidence/README.md` + `evidence/screenshots/`
What to notice:
- Before/after comparisons
- Fail → fix → verify pattern

## 5) The honesty layer
- Read: `docs/STATUS.md` + `docs/SECURITY_AND_IP.md`
What to notice:
- Clear boundaries of what’s public and why
