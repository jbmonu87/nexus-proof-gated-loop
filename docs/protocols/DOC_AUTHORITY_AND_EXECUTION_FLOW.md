# Document Authority & Execution Flow (Public-Safe)

## Why this exists
In fast-moving builds, teams accidentally rewrite the product while “just trying to ship.”
This structure prevents that.

## Two doc types
### A) Binding contracts (Product Truth)
These define what must be true. If execution conflicts, execution must change.

Examples of contract categories:
- interface and interaction rules
- UX invariants (ergonomics, navigation, “physics”)
- fidelity strategy (what “correct” means and how it’s measured)

### B) Execution flow (Work Truth)
These define what we are doing right now and how we run the loop.
They must not reinterpret contracts.

Examples of execution categories:
- current sprint scope
- sprint queue / sequencing
- handoff format
- verification reporting format

## Conflict rule (hard)
If an execution doc conflicts with a contract, the execution doc is wrong.

## Standard execution loop (Plan → Build → Verify → Compound)
1) Plan: define one user-facing outcome + acceptance criteria
2) Build: implement only what’s required
3) Verify: run the proof and publish evidence (pass/fail + artifacts)
4) Compound: if green, promote durable learnings into templates/contracts

## Context health / “rot”
When a session gets long, teams and agents start inventing details.
Triggers to reset:
- repeated confusion about scope or file authority
- inconsistent outputs across turns
- evidence of “forgetting” prior rules

Reset action:
- summarize current state
- restate the governing contracts
- restate the next smallest proof to run
