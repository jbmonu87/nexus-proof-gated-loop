# UX_INVARIANTS.md — Nexus IOE Ergonomics + Interaction Defaults (Tier B)
**Status:** Build Spec (V1)  
**Scope:** Required UX “physics” and interaction defaults across the Shell + all Editors.  
**Relationship to INTERFACE_SPEC.md:**  
- `INTERFACE_SPEC.md` defines **Tier A** (hard UI contract: structure, naming, tab order).  
- This document defines **Tier B** (required ergonomics + interaction invariants).  
**Rule:** Agents MUST satisfy Tier B even if not explicitly requested, so long as Tier A is preserved.
**Enforcement & Phasing:**
- Tier B is **required by Phase 1 Exit**, but may be satisfied incrementally via Atomic Sprints.
- If a Tier B invariant is not in the active sprint scope, it must be recorded as **UX_DEBT** (do not silently implement extra work).
- Tier B work should be validated via a small **Surface Contract** spec under `electron-app/tests/e2e/contracts/` when practical.

---

## 0) Goal
A UI can be pixel-perfect and still be unusable. Tier B exists to prevent that class of failure:
- scrolling that doesn’t scroll
- panels that clip content
- missing right-click menus
- focus/keyboard routing that feels “broken”
- fake tabs / placeholder files that confuse state

---

## 1) Global Interaction Invariants (All Surfaces)

### 1.1 Scrolling & Layout “Physics”
- Any pane that can overflow (Explorer tree, thumbnails, Agent Controller panes like Assistant/Inspector) MUST scroll (`overflow-y: auto`).
- Any scrollable child inside a flex column MUST set `min-height: 0` (and parents must not block it).
- Right panel containers MUST fill height (`flex: 1`, `height: 100%`) so inner scroll can exist.
- The scrollbar must be reachable (no hidden/overlay-only scrollbars that require magic hovering).

### 1.2 Focus & Keyboard Routing
- Clicking a surface (Explorer, Canvas/Grid/Doc, Assistant input, Inspector control) must transfer focus to that surface.
- Global hotkeys (Undo/Redo, Copy/Paste, Find) must route to the **focused** surface.
- Text inputs must not “steal” hotkeys intended for the editor unless the caret is inside an input field.

### 1.3 Selection Model Baseline
- Single-click selects.
- Shift+click extends selection (where multi-select is supported).
- Esc clears selection / closes transient UI (context menus, palette, dropdowns).
- Selection must not silently change due to background renders.

### 1.4 Resizing & Docking
- Explorer (left) is resizable and scrollable.
- Agent Controller (right) is resizable and scrollable (including Assistant + Inspector subpanes).
- Resizing must never collapse the editor canvas into unusable sizes without a visible affordance to restore.

### 1.5 Tab Overflow & Density
- Tabs are one row only. Overflow uses chevrons / a tab list dropdown—never a second row.
- The **Editor Ribbon** is one row only; overflow uses a chevron menu.
- The **Program Menu Bar** is separate from the Editor Ribbon (never duplicated inside the editor surface).

### 1.6 Empty States are Real, Not Fake
- No “fake open files” on launch.
- If nothing is open, show an explicit Welcome / Dashboard empty state.
- If a panel has no content, show a clear empty-state message and action (e.g., “No slides yet → Import PPTX”).

---

## 2) Pointer & Context Menu Contract (Windows-first)

### 2.1 Windows Mapping
- **Right-click** opens context menu on the current target (selection-aware).
- **Double-click** opens/edits the primary content (rename in tree, edit text, open file, etc.).
- **Scroll wheel / trackpad** scrolls the surface under the cursor.

*(Mac parity later: ctrl-click / two-finger click should map to the same context menus, but Windows behavior is the V1 priority.)*

### 2.2 Context Menus are REQUIRED (Tier B)
Context menus MUST exist for:
- Explorer items (files/folders)
- Tabs
- Slide thumbnails
- Canvas objects (PPTX)
- Grid selections (XLSX)
- Document selections (DOCX)
- Optional: Assistant message (copy, quote, etc.)

**Constraints (V1):**
- Keep menus short: 6–10 items max (no deep cascades).
- Every entry MUST map to a Command ID (see `INTERFACE_SPEC.md` / Appendices).
- Context menus must not introduce new feature surfaces not defined elsewhere; they expose existing commands.

### 2.3 Context Menu Universal Items (V1 default)
These items appear when applicable:
- Open / Reveal in Explorer (where relevant)
- Rename
- Duplicate (where supported)
- Delete (with confirmation if destructive)
- Copy / Cut / Paste (where relevant)
- Properties / Info (opens Inspector or a lightweight info popover)

---

## 3) “Agent Freedom” Clause (Prevents Over-Strictness)
If any Tier B invariant is violated (e.g., panel doesn’t scroll), agents are explicitly permitted to:
- change CSS/layout (flex, overflow, heights)
- add missing scroll containers
- add required context menus
- fix focus routing / keyboard handlers

…without needing a user request, so long as:
- Tier A structure/naming/tab order is preserved
- no new major features are invented
- changes are verifiable via a small E2E interaction test (scroll, right-click, focus)

---

## 4) Phase 1 vs Phase 2 Note (Agent Controller)
**Phase 1 (shell + surface affordance):**
- Agent Controller may show a conversation list / "New Chat" affordance, but it may be a stub.

**Phase 2 (functionality):**
- Multiple concurrent chats, optionally with different models, become real and persistable.

Rule: Do not pretend Phase 2 behavior exists unless implemented; Phase 1 may include the UI affordance only.

---

## 5) Ribbon Density & Mode Invariants

### 5.1 Screen Real Estate Principle
The ribbon must maximize canvas space. A sparse "developer UI" ribbon wastes vertical space and signals low production quality.

**Invariants:**
- Ribbon content area MUST NOT exceed 120px height (expanded mode maximum).
- Ribbon MUST use mixed-density layout: hero buttons + 3-row stacks + icon-only buttons.
- Group labels MUST be at the bottom of groups (not inline with controls).
- No more than 2 hero buttons per group.

### 5.2 Mode Persistence
Ribbon mode (compact/normal/expanded) is a user preference that must persist across sessions.

**Invariants:**
- Mode MUST be stored in `localStorage` key `nexus_ribbon_mode_v1`.
- Mode MUST be accessible via View menu (`View > Ribbon: Compact/Normal/Expanded`).
- Mode change MUST be instant (no page reload required).
- Mode MUST apply via `document.documentElement.dataset.ribbonMode` attribute.

### 5.3 CSS Variable Contract
All ribbon dimensions MUST use CSS custom properties for consistent scaling.

**Invariants:**
- Components MUST NOT hardcode pixel values for heights/widths that should scale with mode.
- Variables MUST be defined in `:root` (normal) and `[data-ribbon-mode="compact"]` / `[data-ribbon-mode="expanded"]` selectors.
- Compact mode MUST hide group labels via `--ribbon-show-labels: none`.

### 5.4 Button Density Rules
Ribbon buttons must follow the variant hierarchy to achieve Office-like density.

**Invariants:**
- Hero buttons: vertical layout (icon top, label bottom), `66px` height in normal mode.
- Icon-only buttons: square, `21×21px` in normal mode, tooltip via `title` attribute.
- Small/labeled buttons: `21px` height, icon + text inline.
- 3 stacked rows MUST equal hero button height for visual alignment.
- Dropdowns MUST include a chevron indicator and respect `--ribbon-row-height`.
