# Nexus Proof-Gated Loop — Docs Index

This folder is the **public-safe documentation pack** for Nexus’s proof-gated build loop.
It’s designed to be readable by hiring managers and useful to engineers—without pretending this repo is a full codebase.

## Start here (fastest path)
1. **[INTERVIEW_TOUR](./INTERVIEW_TOUR.md)** — guided walkthrough of what matters and why  
2. **[STATUS](./STATUS.md)** — what’s true right now (and what’s intentionally not here)  
3. **[ARCHITECTURE_AT_A_GLANCE](./ARCHITECTURE_AT_A_GLANCE.md)** — the system in one sitting  

## Core documentation (by intent)
### 1) Build + verification loop
- **[BUILD_LOOP](./BUILD_LOOP.md)** — Plan → Build → Verify → Package → Compound (proof-gated)
- **[VERIFICATION_STYLEGUIDE](./VERIFICATION_STYLEGUIDE.md)** — what counts as “evidence” (determinism, selectors, oracles)

### 2) Protocols (how work is executed)
- **[Start Ritual & Roles](./protocols/START_RITUAL_AND_ROLES.md)** — the “before you touch anything” ritual + agent roles
- **[Doc Authority & Execution Flow](./protocols/DOC_AUTHORITY_AND_EXECUTION_FLOW.md)** — contracts vs flow; what can change and what must not

### 3) Specs (buildable constraints)
- **[Interface Spec](./specs/INTERFACE_SPEC.md)** — UI/UX requirements written as constraints
- **[UX Invariants](./specs/UX_INVARIANTS.md)** — non-negotiable behavior rules that prevent regressions
- **[Fidelity Strategy](./specs/FIDELITY_STRATEGY.md)** — what “correct enough” means and how it’s tested

### 4) Architecture (structure contract)
- **[Nexus Graph](./architecture/NEXUS_GRAPH.md)** — “Project > File” + provenance/lineage as a closed-schema graph

## Folder map
- `architecture/` — structure contracts (e.g., Nexus Graph)
- `protocols/` — rituals, roles, doc authority, execution rules
- `specs/` — binding product constraints (interface, UX invariants, fidelity)
