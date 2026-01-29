# Start Ritual & Roles (Public-Safe)

> Portfolio note: This is a public-safe description of how I run an AI-assisted delivery loop. It is intentionally tool-agnostic and excludes proprietary implementation details.

## Non-negotiable start ritual (before any work)
Before changing code or docs, the active agent/team member must:
1) Re-read the “contracts” that define truth (interface + invariants + fidelity rules).
2) Re-read the current execution state (the active sprint / current focus).
3) Emit a **Brain Scan**: what rules apply to this task and what is explicitly out of scope.

### Brain Scan (required format)
- Read the contracts: YES
- Read current execution state: YES
- What rules apply (3 bullets max):
- Out of scope (3 bullets max):
- Next action: (Plan | Build | Verify | Compound)

## Roles in the loop
### Planner / Plan Reviewer (fail-closed)
- Converts ambiguous intent into a smallest-possible outcome
- Writes acceptance criteria that can be checked
- Blocks scope creep and contract violations

### Builder (implementer)
- Implements only the scoped change
- Prioritizes determinism and testability
- Produces the exact verification command(s) and expected outputs

### Verifier (proof runner)
- Runs the proof exactly as specified
- Reports pass/fail with reproduction steps
- Does not “vibe check” or approve based on appearances

### Researcher (definition-of-ready owner)
- Produces spec packs before deeper work begins:
  - requirements
  - edge cases
  - reference behavior
  - acceptance criteria
- Prevents “build-first, understand-later” churn

### Compounder (system advantage builder)
- Turns repeated lessons into durable rules, templates, and constraints
- **GREEN-first:** only canonize learnings that are supported by proofs
- Keeps the system from re-learning the same lesson every sprint
