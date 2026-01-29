# NEXUS_GRAPH.md — Workspace Lineage Contract (Public-Safe)

**Purpose:** Define the minimal graph model Nexus uses to track provenance (“where did this come from?”), impact (“what breaks if X changes?”), and source-of-truth governance inside a Workspace.

**What this is NOT:** a free-form knowledge graph, a second file system, or a user-drawn whiteboard.  
V1 is deliberately **closed-schema** and primarily **auto-generated** from user actions.

---

## 1) Core Concepts (V1)
- **Workspace (Project):** A container for artifacts + lineage + governance.
- **Routine:** A reusable workflow template (e.g., Monthly Close, Board Deck Refresh).
- **Run:** One execution instance of a Routine (e.g., Jan 2026 Monthly Close).
- **Artifact:** A file or internal representation (deck/sheet/doc/text), plus read-only context items (PDF/image/email/url capture).
- **View:** A publishable, refreshable “slice” of data intended to be embedded elsewhere (e.g., a Live Link table/chart).
- **Decision (optional in V1):** A lightweight “why” anchor used to attach source-of-truth.

---

## 2) Design Principles (Hard Rules)
1. **Single canonical graph:** multiple UI projections are allowed; the underlying model is one.
2. **Closed schema:** node + edge types are fixed enums in V1 (no user-defined types).
3. **Auto-edges first:** most edges are created by actions (import, link, publish view, embed view, export).
4. **Deterministic & diff-friendly:** stable IDs and canonical ordering so changes are reviewable.
5. **Local-first:** the graph is stored with the Workspace (no cloud dependency required).
6. **External safety default:** external artifacts are treated as read-only unless explicitly adopted.

---

## 3) Node Types (V1)
All nodes share: `id`, `type`, `title`, timestamps, optional `tags` and `notes`.

### WORKSPACE
Represents the Project boundary and defaults (e.g., external-read-only behavior).

### ROUTINE / RUN
A Routine defines intent; a Run captures a specific execution and status (`planned | running | completed | aborted`).

### ARTIFACT
Represents an input or output object (office file, internal representation, or context capture).  
May carry an optional “fingerprint” for change detection.

### VIEW
Represents a publishable data slice:
- points to a source Artifact
- contains a stable query/range definition
- has a refresh policy (manual in V1)

### DECISION (optional)
Represents a question/decision being supported, used for governance (source-of-truth mapping).

---

## 4) Edge Types (V1)
### STRUCTURE
- `CONTAINS` : WORKSPACE → (ARTIFACT | ROUTINE | DECISION)
- `INSTANCE_OF` : RUN → ROUTINE

### LINEAGE / PROVENANCE
- `DERIVES_FROM` : ARTIFACT → ARTIFACT (import/export/transform/copy/compile)
- `USES` : (ROUTINE | RUN) → (ARTIFACT | VIEW)
- `PRODUCES` : (ROUTINE | RUN) → ARTIFACT
- `PUBLISHES_VIEW` : ARTIFACT → VIEW
- `RENDERS_IN` : VIEW → ARTIFACT (where a view is embedded/consumed)

### AUTHORITY (Governance)
- `SOURCE_OF_TRUTH_FOR` : (ARTIFACT | VIEW) → DECISION  
  V1 constraint: within a Workspace, **one** source-of-truth per decision/metric scope.

---

## 5) Invariants (Anti-Sprawl Rules)
1. **Acyclic lineage:** `DERIVES_FROM` must form a DAG (no cycles). Cycles are refused with a safe alternative (e.g., create a new artifact version).
2. **No orphan bindings:** embedded views must resolve: `VIEW → source ARTIFACT` must exist.
3. **Blast radius is computable:** given any Artifact change, the system can enumerate downstream impact:
   - `ARTIFACT → VIEW → consuming ARTIFACTs`
   - `ARTIFACT → derived ARTIFACTs`
4. **Deterministic serialization:** nodes/edges are stored/exported in canonical order to minimize diff churn.

---

## 6) Why this matters (the “PM proof”)
This model is the backbone for:
- answering “what’s the source of truth?”
- preventing silent drift and broken links
- making refresh + reuse safe in a “Project > File” world
- enabling agents to reason over provenance deterministically (no vibes)

