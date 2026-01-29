# Fidelity Strategy: The Ten Pillars
**Status:** Living Document
**Applies To:** All Engines (Slide, Grid, Doc)

To escape "Phase 1 Hallucinations," Nexus adopts the **Lead Import Architect** standard. We value **Correctness over Aesthetics** and **Honesty over Heroics**.

## Definitions (Applies to all pillars)

- **Source file (external):** The original Office file outside Nexus. Nexus MUST treat this as immutable unless the user explicitly chooses overwrite/export-in-place.
- **Project copy (editable):** The Office file inside the Project folder that Nexus opens and edits in V1.
- **Engine Model (internal):** The in-memory representation used for rendering/editing (may be IR, OOXML DOM, or patch-layer).
- **Ledger Statuses (canonical):**
  - `RENDERED` — visible and editable in Nexus.
  - `APPROXIMATED {delta}` — visible but with known fidelity loss.
  - `PRESERVED_RAW` — not rendered/editable, but preserved for round-trip.
  - `DROPPED {reason}` — not preserved (should be rare and intentional).
  - `ERROR {reason}` — parse/resolve failure.

## 1. Import Fidelity Ledger (The Foundation)
- **Requirement:** Every `import` action produces an Import Fidelity Ledger under `.nexus/` (ignored by git), keyed by `relativePath`:
  - Example: `.nexus/ledgers/import/<relativePath>.audit.json`

- **Contract:** Iterate *every* significant element in the source package (OpenXML + relationships), and record one canonical status:
  - `RENDERED`
  - `APPROXIMATED {delta}`
  - `PRESERVED_RAW`
  - `DROPPED {reason}`
  - `ERROR {reason}`

- **Rule:** Ledger entries MUST include enough location context to debug missing content:
  - slide/sheet/page index (where applicable)
  - part path (e.g., `/ppt/slides/slide3.xml`)
  - relationship ids (media/fonts/themes)
  - resolved master/layout/theme references (PPTX)

### 1A) “Accounted For” Gate (Prevents Hallucinations)
- **Gate:** For V1, the hard requirement is:
  - `DROPPED == 0` (account for everything via render/approx/preserve)
- **Must-Have Feature Gate:** For declared “Must Have” features, require:
  - `PRESERVED_RAW` is NOT acceptable (must be `RENDERED` or `APPROXIMATED`).
  - Example: PPTX backgrounds + basic text boxes are “Must Have.”

### 1B) Source Immutability vs Project Editability (Consistency with V1)
- Import MUST NOT mutate the **external Source file**.
- In V1, the **Project copy** is editable and may be mutated by Save.
- Ledger MUST clearly state:
  - `source_path` (external, immutable by default)
  - `project_path` (editable copy)

## 2. Export Fidelity Ledger (The Mirror)
- **Requirement:** Every `export` action produces an Export Fidelity Ledger under `.nexus/`:
  - Example: `.nexus/ledgers/export/<relativePath>.audit.json`

- **Contract:** Export MUST report outcomes using the same canonical status set as Import:
  - `RENDERED` — emitted from the editable model.
  - `APPROXIMATED {delta}` — emitted with known loss.
  - `PRESERVED_RAW` — re-emitted untouched (round-trip preservation).
  - `DROPPED {reason}` — intentionally removed (requires explicit user choice).
  - `ERROR {reason}` — failed to emit.

- **Minimum-Change Principle (Critical for round-trip):**
  - Export SHOULD rewrite only the parts that Nexus actually edited.
  - Everything else MUST remain byte-preserved where feasible (especially `PRESERVED_RAW` parts + their relationships).

### 2A) Non-Destructive Export (Source Immutability)
- **Rule:** Export MUST NOT overwrite the original imported Office file by default.
- **Default behavior:** Export creates a new output file (timestamped or user-named).
- **Overwrite behavior:** Allowed only with an explicit confirmation step.

### 2B) Two Export Modes (Make the choice explicit)
1) **Round-trip Preserving Export (default)**
   - Re-emit `PRESERVED_RAW` parts untouched.
   - If an edit conflicts with preserved content, export MUST:
     1) keep preserved content intact,
     2) flag the conflict in the export ledger,
     3) require an explicit user choice for destructive resolution.

2) **Sanitized/Compatibility Export (optional)**
   - May drop preserved raw elements.
   - MUST show a warning + mark losses as `DROPPED {reason}`.

## 3. Stable IDs & Canonicalization
*   **Requirement:** Every IR element needs a UUID (`nexus_id`) that persists across:
    *   Import -> Save -> Export -> Import (Round Trip).
    *   Minor edits (moving a shape shouldn't change its ID).
*   **Normalization:** IR must be deterministic. No random map ordering.
*   **Why:** Enables reliable Diffing and "Smart Update" (only syncing changed slides).

## 3A. Graph Integrity & Lineage (Nexus Graph)
Fidelity is not only “object mapping.” It also requires **provenance correctness**: Nexus must know (and prove) what produced what.

**Requirements:**
- **Stable IDs apply to graph nodes/edges** (not just IR elements). A chart/view embedded in a slide must remain traceable across Import → Edit → Export.
- **Deterministic serialization:** exporting the graph (JSON snapshot) must be canonical (stable ordering) to enable diffs and trustworthy agent context.
- **No orphan bindings:** a Live Link consumer (slide object) must always resolve to a `VIEW`, which must resolve to a `source_ref` artifact.
- **Blast radius is computable:** given a changed artifact, Nexus can enumerate downstream impacted outputs via lineage traversal.
- **Acyclic lineage:** derivation edges must not cycle (cycles destroy provenance reasoning).

**Verification (tests):**
- Given a fixture workspace with Live Links, assert that:
  1) every embedded view has a resolvable `source_ref`,
  2) refresh updates the “as-of” fingerprint deterministically, and
  3) graph export is byte-stable across no-op saves.

## 4. Round-Trip Verification (IR -> OOXML -> IR)
*   **Requirement:** A test suite asserting *Structural Equivalence*, not just "No Crash".
*   **Method:**
    1.  Import `A.pptx` -> `IR_1` + `Ledger_1`.
    2.  Export `IR_1` -> `B.pptx`.
    3.  Import `B.pptx` -> `IR_2` + `Ledger_2`.
    4.  **Assert:** `IR_1 ≈ IR_2` (Tolerance for floats/IDs).
*   **Why:** The ultimate proof of "Lossless Editing."

## 5. Semantic Visual Diff (Geometry-Aware)
*   **Requirement:** Move beyond Pixel Diffs (brittle) to Semantic Diffs.
*   **Checks:**
    *   Bounding Box coverage (overlap %).
    *   Text Metrics (line count, overflow boolean).
    *   Z-Order hierarchy matches.
*   **Fallback:** Pixel diffs are only for "final sanity check."

## 5A. Reference Renderer Diff (Bootstrap, Low Manual Work)
- **What it is:** Generate “expected” slide images from a pinned external renderer (e.g., LibreOffice) and compare Nexus output against it.
- **Why it helps:** During churn, it avoids hand-curating baselines and reduces the need for many narrow tests.
- **How we keep it clean (no debt):**
  - Cache reference outputs in `.nexus/reference-renders/` (never committed).
  - When we’re confident, snapshot the best reference outputs into `electron-app/tests/e2e/oracles/` in an Aesthetic/Oracle sprint and stop generating them at test time.

## 6. Honest UI (Unsupported Banners)
*   **Requirement:** If `Ledger.dropped > 0`, the UI **must** show a warning banner.
*   **Message:** "This slide contains unsupported features: SmartArt, 3D Charts."
*   **Why:** Trust. Users accept limitations if stated; they quit if surprised.

## 7. Fixture Zoo & Scoring
- **Requirement:** A rigorous "Torture Suite" of files (Financial Models, Legal Contracts, Marketing Decks).

### 7A) Two Scores (Don’t confuse preservation with rendering)
1) **Accounted-For Score (Phase 1 honesty)**
   - `(RENDERED + APPROXIMATED + PRESERVED_RAW) / TOTAL * 100`
   - Goal: 100% (i.e., `DROPPED == 0`)

2) **Render Fidelity Score (product usefulness)**
   - `(RENDERED + APPROXIMATED) / TOTAL * 100`
   - Track per engine (PPTX/XLSX/DOCX) and per feature class (text, backgrounds, charts).

- **Trend:** Graph both scores per sprint.

## 8. Preservation "The Black Box" (Lossless Round-Trip)
*   **Requirement:** Any OpenXML element not understood by Nexus (e.g., SmartArt, Legacy Charts, Macros) MUST be preserved as a raw blob.
*   **Mechanism:** Store in `unsupported_blobs` store attached to the Sidecar (not the DOM).
*   **Export:** Inject these blobs back into the OOXML tree on export. "What we don't know, we don't touch."

**Relationship Preservation (REQUIRED):**
- Preserved elements MUST include required relationship parts:
  - media relationships (images, embedded objects)
  - theme/master/layout dependencies (PPTX)
  - chart parts + embedded workbook parts (where applicable)
- If a preserved element cannot be reattached due to missing relationships, export MUST mark `ERROR` (not silently downgrade).

## 9. Text Metrics Parity (The Font Service)
*   **Requirement:** Layout fidelity requires metric-compatible fonts.
*   **Mechanism:**
    *   Extract embedded fonts from PPTX.
    *   Map system fonts to metric-identical Web Fonts (e.g., `Arial` -> `Liberation Sans`).
    *   Use a backend Text Measurement Service (Headless Chrome or Skia) if Canvas metrics diverge > 1%.
*   **Why:** Prevents "Text Wrap Rage" (where a 2-line title wraps to 3 lines and pushes content off-slide).

## 9A. Text Layout Contract (Stop Wrap Drift Early)
- **Requirement:** For a small set of known text boxes, Nexus must match:
  - **Line breaks** (how many lines + the actual line text)
  - **Overflow flag** (is text spilling outside the box)
- **Why:** Text wrap bugs create “invisible churn” across many slides; one strong contract test prevents dozens of later micro-tests.
- **Mechanism:** Use a single debug seam (`__NEXUS_DEBUG__.pptx.getTextLayout`) and compare against the reference renderer output during S09M.

## 10. Interaction Fidelity (The Usability Invariant)
- **Requirement:** Visual match is insufficient. The *physics* must match.
- **Source of truth:** `docs/UX_INVARIANTS.md`

**Invariants (minimum):**
- **Scroll:** If content overflows, it MUST scroll.
- **Focus routing:** Hotkeys route to the focused surface (not “whatever”).
- **Editability gate:** If it looks like a text box, it MUST be clickable and editable; otherwise it MUST be explicitly marked as `APPROXIMATED` or `PRESERVED_RAW` in the Import Ledger (no silent failure).
- **Resize:** Panels MUST respond to window resizing (no clipped content).

**Verification:** E2E tests must `click`, `type`, and `scroll`, not just `expect(locator).toBeVisible()`.
