# Sanitized Excerpts (Edited from Internal Docs)

This file is a **public-safe** set of excerpts from internal documentation.
Edits remove proprietary details, file paths, and any sensitive fixtures.

The point is not the specific product. The point is the *method*: define correctness, prove it, and only then compound.

---

## Excerpt: “If it can’t be verified, it doesn’t exist.”
- Any behavior that matters must have an explicit proof path.
- If the team can’t reliably check it, we treat it as *not implemented*.
- “Looks right” is not a definition of done.

---

## Excerpt: Atomic sprint rule (scope control)
Each sprint must have:
1) **One user-facing outcome**
2) **One proof** that fails first and goes green

No proof = no promotion.

Only compound on green:
- green proof → promote scope
- red proof → fix or shrink scope

---

## Excerpt: Example UX invariant (concrete, human-readable)
**Invariant:** Selecting a slide thumbnail must select that slide — not the last slide, not a random slide.

**User expectation**
- When the user clicks thumbnail *N*, the editor focuses slide *N*.

**Failure mode (example)**
- Any click inside the thumbnail pane navigates to the final slide.

**Proof shape (conceptual)**
- Action: click thumbnail 4
- Expectation: active slide id == 4
- Guardrail: selector and state read are deterministic (no timing-based guessing)

---

## Excerpt: Test taxonomy (conceptual)
To make correctness legible, proofs fall into three buckets:

- **Contracts:** ergonomics / invariants (things that must always behave the same)
- **Features:** command behavior (specific user actions mutate state)
- **Oracles:** fidelity comparisons (visual correctness against baselines)

---

## Excerpt: Safety boundary for AI-assisted building
AI assistance is valuable, but must operate within explicit constraints:
- Planner defines scope + acceptance criteria
- Builder implements only the scoped change
- Verifier blocks promotion unless proof passes

Confidence is not evidence. Proof is evidence.
