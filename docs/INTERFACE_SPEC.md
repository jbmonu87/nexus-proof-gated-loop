# INTERFACE_SPEC.md — Nexus IOE (App Menu + PPTX/XLSX/DOCX Ribbon + Inspector Contract)
**Status:** Build Spec (V1)  
**Scope:** Binding UI contract for **Program Menu**, **Command Palette**, **in-editor Ribbon Tabs**, and the **Inspector** for PPTX/XLSX/DOCX.  
**North Star:** “Project > File” + “Visual Correctness” + “Proposal-first AI” (generate to ~90%, human polishes last 10–20%).  
**Non-goal (V1):** Macro ecosystems, Office add-ins, niche legacy parity, and anything that steals vertical space from the editor canvas.

---

## 0) Definitions (to prevent UI hallucinations)
- **Program Menu (Top Menu Bar):** App-level menus above everything (VS Code / Antigravity style). Controls projects, windows, agents, settings. **Does not** contain document formatting controls.
- **Editor Ribbon (In-Editor Tabs):** Per-file-type ribbon inside the center pane (PPTX/XLSX/DOCX). This is where formatting, insert, and file-specific tools live.
- **Agent Controller (Right Panel):** The right-side control surface with two tabs: **Assistant** and **Inspector**.
- **Assistant (Agent Controller tab):** Chat + proposals + diffs + “apply/merge” controls.
- **Inspector (Agent Controller tab):** Persistent selection-based properties + precision tools. Faster than hunting ribbon buttons; the human “polish surface.”
- **Command Palette:** Global search surface to run *any* command (app + editor) by name.
- **Command:** Named action with stable id (e.g., `pptx.align.left`, `ai.deck.generateFromBrief`). A command may appear in menu, ribbon, inspector, context menu, or palette.
- **Context Menu:** Selection-aware right-click menu exposing existing commands. See `docs/UX_INVARIANTS.md`.

---

## 1) Non-Negotiable UX Principles
1. **Space is sacred.**  
   - Must be: program menu (single row) + tabs (single row) + **one ribbon region** (with two internal rows: ribbon tabs row + ribbon toolbar row)
   - Forbidden: any *global* ribbon below the program menu
2. **Consistent tab order across file types.**  
   - Always: **File → Home → Insert → (Type-specific) → Review (AI+Collab) → View**
3. **AI is not a “nice-to-have sidebar.”** It’s a first-class production tool:
   - **Generate** (create 80%+ of the artifact)
   - **Refine** (polish to client-ready)
   - **Verify** (catch errors, inconsistencies, layout issues)
4. **Proposal-first for high-impact changes.**  
   - Small atomic edits can be immediate + undo.  
   - Multi-step changes default to a **Change Set Card** with preview + visual diff + merge controls.
5. **Inspector is the polish engine.**  
   - Position/Size/Style tokens live here across PPTX/XLSX/DOCX.  
   - Ribbon focuses on creation + broad operations.

### 1.1 UI Ergonomics + Interaction Invariants (Tier B)
> **Status:** REQUIRED.  
> **Source of truth:** `docs/UX_INVARIANTS.md`  
> **Rule:** Agents MUST satisfy Tier B invariants (scrolling, focus routing, context menus, empty states) while preserving Tier A.

---

## 2) Global Shell Contract (All File Types)

### 2.1 Program Menu (Top Menu Bar) — REQUIRED
**Exactly:** `File · Edit · View · Project · Agents · Tools · Window · Help`  
**Rule:** anything that changes document content belongs in the *in-editor* ribbon/inspector, not here.

#### File
- New Project…, Open Project…, Add Folder to Project…
- Open File…, Import…
- New: Document (DOCX), Presentation (PPTX), Spreadsheet (XLSX)
- Save, Save As…, Save All
- Export… (delegates to active editor)
- Close File, Close Project
- Project Settings…, Preferences/Settings…
- Print… (delegates), Exit/Quit

#### Edit
- Undo, Redo
- Cut / Copy / Paste, Paste Special…
- Select All
- Find…, Replace…
- Find in Project…
- Rename Item… (contextual: slide titles, named ranges, headings)

#### View
- Command Palette…
- Quick File…
- Toggle Left Pane, Toggle Right Pane
- Toggle Inspector, Toggle Assistant
- Toggle Status Bar
- Fullscreen, Zen Mode
- App UI Zoom In/Out/Reset
- Theme: Light/Dark/System

#### Project
- Project Overview
- Index/Reindex Project
- Open Context (CONTEXT.md)
- Open Brain (NEXUS_BRAIN.md)
- Fidelity Reports (import/export ledgers)
- Diff Viewer (semantic diffs)
- Snapshots: Create / Restore

#### Agents
- Mode: Manual / Semi-Auto / Auto (policy-gated)
- Propose Changes (no mutation)
- Apply Proposed Changes
- Review Queue (pending proposals)
- Run Workflow…
- Agent Logs
- Safety / Permissions…

#### Tools
- Extensions/Plugins… (V1 can be minimal)
- Keyboard Shortcuts…
- Diagnostics (perf, font load, thumbnail pipeline)
- Developer Tools (flagged / optional)
- Developer Tools toggles “Developer Mode”. In normal user mode, debug seams (e.g., `window.__NEXUS_DEBUG__`) MUST be absent/undefined.

#### Window
- New Window
- Split Editor
- Next/Previous Tab
- Focus Left/Editor/Right

#### Help
- Quickstart, Documentation
- Report a Bug
- Check for Updates
- About Nexus

---

### 2.2 Command Palette — REQUIRED
**Behavior**
- Fuzzy search across **all commands**
- Context-aware ranking (active file type + selection)
- Shows short descriptions
- Supports “run with args” where relevant (e.g., “Set Zoom 125%”)

---

### 2.3 Canonical Layout + Visual Style Contract (REQUIRED for Mockups + UI Tests)

**Purpose:** Prevent UI hallucinations. This section is the binding “golden layout” for image generation and for automated render tests.

#### 2.3.1 Window + Chrome (Antigravity-style)
- **Mode:** Dark theme (default for mockups).
- **Top row (single):** Program Menu integrated into the app chrome row.
  - Left: small Nexus mark + menu items exactly: `File · Edit · View · Project · Agents · Tools · Window · Help`
  - Center: optional app title text `Nexus IOE` (or file-type label in parentheses optional)
  - Right: standard window controls.
- **No global ribbon** under the program menu (ever).

#### 2.3.2 Editor Tabs Row (single row; REQUIRED)
- One row of file tabs directly below the program menu.
- Tabs show: file-type icon + filename + close “×”.
- **Exactly three open tabs (for mockups):**
  1) `Nexus Vision.docx`
  2) `Nexus Pitch Deck.pptx`
  3) `Nexus Metrics Model.xlsx`
- Active tab styling: brighter text + subtle underline/accent. No wrapping to a second row.

#### 2.3.3 3-Pane App Layout (REQUIRED)
- Left: **Explorer / Project Tree** (fixed width; no reflow between file types).
- Center: **Editor** (in-editor ribbon + canvas).
- Right: **Right Panel** with two top tabs: `Assistant` | `Inspector`.
  - Default for mockups: **Inspector active**.
  - Assistant exists but is not the active panel in baseline images.

#### 2.3.4 Left Explorer Contract (REQUIRED; identical across PPTX/XLSX/DOCX mockups)
- Title: `Explorer` (or `File Explorer (Project Tree)`; pick one and keep consistent across all mockups).
- Project root name: `Nexus Pitch (Project)` (consistent).
- Canonical tree (expanded):
  - `Deck/` → `Nexus Pitch Deck.pptx`
  - `Model/` → `Nexus Metrics Model.xlsx`
  - `Memo/` → `Nexus Vision.docx`
  - `Assets/` → (optional; may show `att1ts.png` or similar)
- Selected item in tree matches the active file tab.

#### 2.3.5 In-Editor Ribbon Row Rules (REQUIRED)
- Ribbon has **two layers** inside the editor area:
  1) **Ribbon Tabs** row (text tabs, single row).
  2) **Ribbon Toolbar** row (icons + compact controls), single row.
- Toolbar groups may show group labels (e.g., Clipboard, Font) but must remain **one row** (use overflow chevron if needed).
- **Never invent extra ribbon tabs** beyond the file-type contract.
  - PPTX tabs: `File · Home · Insert · Design · Arrange · Review · View`
  - XLSX tabs: `File · Home · Insert · Data · Formulas · Review · View`
  - DOCX tabs: `File · Home · Insert · Layout · References · Review · View`

#### 2.3.6 Right Panel (Inspector) Visual Contract (REQUIRED)
- Top toggle: `Assistant` and `Inspector` tabs; **Inspector active** in baseline mockups.
- Inspector uses collapsible section blocks with clear headers and compact controls.
- A small Nexus “spark/star” watermark may appear at the bottom-right of the right panel (optional but consistent if used).
- Inspector content is **selection-aware**, but baseline mockups must show the canonical sections for each file type (see 2.4).

---

### 2.4 Canonical “Home” Mockup Fixtures (REQUIRED for Image Generation + Render Tests)

**Rule:** These fixtures are the *golden references* for (a) VC screenshots and (b) automated visual regression tests.

#### 2.4.1 PPTX Fixture (Home tab)
- Active file: `Nexus Pitch Deck.pptx`
- Ribbon active tab: `Home`
- Left thumbnail strip shows **6 slides** (Problem, Solution, Product, Traction, Ask; exact titles can vary but must be business-realistic).
- Main slide canvas shows a clean “Nexus IOE” pitch slide (title + 3-column value props; simple diagram allowed).
- Inspector (PPTX) must visibly include at least:
  - Position (Align/Distribute/Layer order)
  - Size (W/H + lock ratio)
  - Typography
  - Fill/Stroke with **theme color grid** + recent colors
  - Arrange (group/rotate/order) (can be collapsed but present)

#### 2.4.2 XLSX Fixture (Home tab)
- Active file: `Nexus Metrics Model.xlsx`
- Ribbon active tab: `Home`
- Sheet shows a plausible growth/financial model table (quarters across columns; Revenue/COGS/Gross Margin/Opex/EBITDA rows).
- A cell range is highlighted; formula bar visible with an example formula (e.g., `=SUM(C4:F4)`).
- Inspector (XLSX) must visibly include at least:
  - AI Actions (Explain/Write Formula, Clean Data, Summarize)
  - Cell/Range Formatting (Number format, font, alignment, borders, fill)
  - Live Link Publish section with a primary button `Publish Selection as View`

#### 2.4.3 DOCX Fixture (Home tab)
- Active file: `Nexus Vision.docx`
- Ribbon active tab: `Home`
- Document shows a memo titled `Nexus IOE — Vision & Mission` with headings + bullets; cursor visible in body text.
- Page is centered with paper margin and subtle shadow; optional outline/navigation visible.
- Inspector (DOCX) must visibly include at least:
  - AI Actions (Rewrite for clarity, Tighten/Shorten, Expand with Evidence, Summarize)
  - Styles (style selector + promote/demote)
  - Typography (font/size/color)
  - Outline/Navigation section (headings list)

---

### 2.5 Mockup/Test Rule: “No Hallucinated Tabs”
If a generated image shows ribbon tabs not listed in this spec (e.g., `Collaboration`, `Help`, `Page Layout` in XLSX), that image is **invalid** and must be regenerated.

---

## 3) Behavioral Support (The "Alive" Contract)
**Principle:** The UI must not only *look* correct (Pixels) but *feel* standard (Behavior).

### 3.1 Shell Interactions
*   **Menus:** Global Menu items (`File`, `Edit`) must verify the `activeEngine` and dispatch commands to it. They are not static decorators.
*   **Panels:** 
    *   **Left (Explorer):** Must be **Scrollable** (`overflow-y: auto`) and **Resizable** (drag handle).
    *   **Right (Assistant/Inspector):** Must be Resizable. Default width ~300px.
*   **Focus Management:** 
    *   Clicking a canvas (Slide/Grid) must transfer focus to the engine. 
    *   Global Hotkeys (Ctrl+Z, Ctrl+C) must route to the *focused* surface (Editor vs. Input Field).

### 3.2 Visual Handoffs (Thumbnails & Icons)
*   **Thumbnails:** Must use `toDataURL()` or similar to render *actual* content. No generic placeholders in the slide strip.
*   **Icons:** Use generic system icons (File, Folder, PPTX) unless a specific "Gold Standard" test demands an exact asset match. Do not hardcode "Vision.docx" icons.


### 3.3 Runtime Defaults vs Fixture Defaults
*   **Runtime Default (Product):** Agent Controller defaults to the **Assistant** tab.
*   **Fixture Default (Gold Screenshots):** Inspector tab is active (only for baseline images/tests).
*   **Rule:** Tests must explicitly specify which mode they expect (fixture vs runtime). No ambiguity.

### 3.4 Functional Gates (Behavioral Checklist)
*   **Menu Dispatch:** Global Menus (`File`, `Edit`) must route commands to the `activeEngine`.
*   **Undo Stack:** `Ctrl+Z` must trigger the **Engine's** undo stack, not just a browser undo. Stacks must not cross-contaminate between tabs.
*   **Panel Interactive:** Left panel must be Scrollable + Resizable. Right panel must be Resizable.
*   **Toggle Integrity:** Buttons like "Outline Toggle" must exist and function.
*   **Thumbnail Truth:** Thumbnails must be real renders (`toDataURL`), not placeholders.
*   **PPTX Text Editability:** Double-click a text box enters edit mode (caret visible). Typing edits text. Click-away commits. Esc cancels. Ctrl+Z undoes. Inspector Typography updates to reflect selection.

---

## 4) Cross-Editor Ribbon Contract (Applies to PPTX/XLSX/DOCX)
### 4.1 Standard Tab Order (REQUIRED)
1. **File**
2. **Home**
3. **Insert**
4. **Type-Specific Tabs** (varies)
5. **Review** *(combined Collaboration + AI)*
6. **View**

### 4.2 AI Surfaces (REQUIRED)
AI tools appear in **two places**:
1. **Home tab:** fast “create/refine” actions (front-and-center)
2. **Review tab:** deeper QA + verification + comment integration

**AI actions must declare scope**: `Selection | Slide/Sheet/Section | Whole File | Project`.

### 4.3 “Generate to 90%” Contract (REQUIRED)
Each file type must expose an obvious top-level action:
- **PPTX:** `AI: Generate Deck…`
- **XLSX:** `AI: Generate Model…`
- **DOCX:** `AI: Generate Doc…`

**Interaction pattern (V1):**
- Opens a **Brief Card** in Assistant with 5 fields:
  1) Audience + decision
  2) Output type + length (e.g., 10-slide deck; 4-tab model; 2-page memo)
  3) Sources (project files / pasted links / "none yet")
  4) Constraints (tone, brand tokens, definitions)
  5) "Done means…" acceptance criteria
- AI produces **Draft + Change Set Card** (proposal)
- User merges, then polishes with Inspector + atomic tools

### 4.4 Ribbon Visual Specification (CSS Variable Mode System)

**Purpose:** Define the "Gold Standard" dense ribbon layout for professional Office-like UI density. Screen real estate is expensive—sparse ribbons waste space.

#### 4.4.1 Dimension Contract (CSS Variables)
The ribbon uses CSS custom properties for responsive sizing across three modes:

| Variable | Compact | Normal (default) | Expanded |
|----------|---------|------------------|----------|
| `--ribbon-content-height` | 64px | 100px | 120px |
| `--ribbon-hero-height` | 48px | 66px | 84px |
| `--ribbon-stack-height` | 48px | 66px | 84px |
| `--ribbon-row-height` | 15px | 21px | 26px |
| `--ribbon-icon-size` | 15px | 21px | 26px |
| `--ribbon-hero-icon` | 18px | 22px | 28px |
| `--ribbon-show-labels` | none | block | block |

**Mode Activation:** Set `document.documentElement.dataset.ribbonMode` to `compact`, `normal`, or `expanded`.

**Persistence:** Store in `localStorage` key `nexus_ribbon_mode_v1`. View menu commands: `view.ribbon.compact`, `view.ribbon.normal`, `view.ribbon.expanded`.

#### 4.4.2 Button Variant Rules
**Mixed-density layout pattern:**

1. **Hero Buttons** (`66px` in normal mode)
   - Anchor each group (max 2 per group)
   - Vertical: icon on top, label below
   - For high-frequency actions: Paste, New Slide, AI Rewrite, Insert Text Box

2. **Stacked Buttons** (3 rows × `21px`)
   - Secondary actions organized in vertical stacks
   - 3 items per stack fills hero button height
   - Small label optional, icon required

3. **Icon-Only Buttons** (`21×21px`)
   - Formatting toggles: Bold/Italic/Underline, alignment
   - No text labels—tooltip via `title` attribute
   - Use for high-density control rows

4. **Dropdowns** (`21px` height)
   - Font family (wide: 90px), font size (narrow: 44px)
   - Include chevron indicator

#### 4.4.3 Group Organization Pattern
```
RibbonGroup (label at bottom)
├── HeroBtn (optional, max 2)
├── RibbonStack
│   ├── RibbonRow → IconBtn | SmallBtn | Dropdown
│   ├── RibbonRow
│   └── RibbonRow
└── RibbonSep (optional vertical separator)
```

**Group labels:** 9px font, color `#999`, centered below content. Hidden in compact mode via `display: var(--ribbon-show-labels)`.

#### 4.4.4 Per-File-Type Home Tab Layout (REQUIRED)

**PPTX Home:** Clipboard (Paste hero + Cut/Copy stack) | Slides (New Slide hero + Dup/Del) | Font (dropdowns + B/I/U/S icons) | Paragraph (alignment + bullets) | Insert (Text Box hero + Image/Shape icons) | AI (Rewrite hero + AI stack)

**XLSX Home:** Clipboard | Font (family/size dropdowns + B/I/U) | Alignment (L/C/R + wrap) | Number ($/% icons + decimal) | Cells (insert/delete) | AI (Write Formula hero + Clean/Summarize)

**DOCX Home:** Clipboard | Styles (dropdown) | Font (family/size + B/I/U) | Paragraph (alignment + lists + indent) | Insert (Image/Table/Link) | AI (Rewrite hero + Tighten/Expand)

#### 4.4.5 AI Group Styling
AI groups use a subtle glow effect to distinguish them:
- Background: linear gradient with `rgba(168, 199, 250, 0.03)` to `rgba(168, 199, 250, 0.08)`
- Border: `rgba(168, 199, 250, 0.15)` on left/right
- Icon/text color: `#a8c7fa` (AI blue)
- Label color: `#8ab4f8`

---

## 5) PPTX (Presentation) — Ribbon + Inspector (V1)

### 5.1 PPTX Tabs (REQUIRED)
**File · Home · Insert · Design · Arrange · Review · View**

#### 5.1.1 PPTX Import / Fidelity / Export (REQUIRED)

**Principle:** PPTX handling is **lossless at the package level** and may be **lossy at the render/edit level**.
- Nexus MUST import the full `.pptx` package (all parts + relationships).
- Nexus MAY only render/edit a subset in V1, but MUST preserve the rest for round-trip export.

#### Import/Export Fidelity Ledger (PPTX)
See `FIDELITY_STRATEGY.md` (Pillars 1–2, 8) for the canonical status taxonomy and preservation/export rules. PPTX must comply.

Ledgers MUST include enough information to debug “missing background/text”:
- slide index + part path
- relationship ids for media/fonts
- master/layout/theme references used (or missing)

#### Export Policy (PPTX) — Default MUST Preserve
Default export mode is **Round-trip Preserving Export**:
- Elements marked `PRESERVED_RAW` MUST be re-emitted unchanged.
- Elements marked `RENDERED` or `APPROXIMATED` may be rewritten only where Nexus edits occurred.
- If an edit would conflict with an unrendered preserved element (e.g., moving objects on a slide that contains preserved SmartArt), Nexus MUST:
  1) keep preserved element intact, and
  2) flag the conflict in the export ledger, and
  3) require an explicit user choice for any destructive resolution.

Optional (explicit) export mode: **Sanitized/Compatibility Export**
- Allows dropping `PRESERVED_RAW` elements.
- MUST show a preview warning and mark all losses as `DROPPED` in the export ledger.

#### Black Box Preservation (V1)
Unsupported elements MAY be preserved as raw OOXML blobs or untouched parts/relationships.
- UI MUST disclose this via Appendix F banners.
- UI MUST expose “View Preserved Elements” (read-only list) with slide locations.

### 5.1.2 PPTX “Light Master” Contract (REQUIRED V1)

**Goal:** Imported decks should look “the same enough” without implementing full PowerPoint master editing.

Nexus MUST support a **Light Master** model sufficient for visual correctness:
1. **Slide Size + Units**
   - Read slide size and render with correct aspect ratio and margins.

2. **Theme Tokens (baseline)**
   - Import theme color scheme + font scheme.
   - Map to Nexus theme tokens (theme-first rendering and pickers).

3. **Background Fidelity**
   - Apply slide background correctly, including:
     - master/layout background inheritance
     - solid/gradient fills (V1 may approximate gradients but must render something close)
     - background images (resolve relationships, crop/fit)

4. **Layout + Placeholder Geometry**
   - Resolve layout placeholders (title/body/etc.) to concrete geometry and default text styling.
   - Shapes/text should inherit the effective style (theme + master + layout + direct formatting).

5. **Theme Extraction / Brand Kit (agent-friendly)**
   - Provide an agent action to propose a Theme Token Set extracted from the imported deck (`AI: Apply Brand Kit…`).
   - This produces a proposal-first Change Set Card (no silent theme mutation).

**Non-goal (V1):** Full master editor UI.
**Requirement (V1):** Master/layout/theme must still be applied so slides render correctly.

---

### 5.2 PPTX — FILE Tab (REQUIRED)
**File Actions**
- New / Open / Close (contextual)
- Save, Save As…, Save a Copy…
- Export: PDF, PNG/JPG (slides), Handout (PDF)
- Share (copy link / export bundle) *(optional V1)*
- Document Info: title, author, last export timestamp

**AI (Front Door)**
- **AI: Generate Deck…** *(whole file; proposal-first)*
- **AI: Generate Slide…** *(current slide; proposal-first)*
- **AI: Build from Outline…** *(outline → slides; proposal-first)*
- **AI: Apply Brand Kit…** *(maps tokens; proposal-first)*

## 5.2.1 PPTX Text Editing Contract (REQUIRED V1)

**Minimum behavior:**
- Single-click selects a text box (object selection).
- Double-click (or Enter) enters text edit mode (caret).
- Drag selects text; Shift+Arrow extends selection.
- Ctrl+B / Ctrl+I / Ctrl+U apply emphasis to selection when supported.
- Ctrl+Z / Ctrl+Y operate on the PPTX engine undo stack (not browser undo).
- If a text element is visible but not editable, it MUST be flagged in the Import Fidelity Ledger as `APPROXIMATED` or `PRESERVED_RAW` (never “silent failure”).

**V1 scope:** Basic rich text runs (bold/italic/underline, size, color) and paragraph basics (bullets, alignment).

---

### 5.3 PPTX — HOME Tab (REQUIRED)
### Clipboard
- Paste, Cut, Copy, Format Painter

### Slides
- New Slide (layout dropdown)
- Duplicate Slide
- Delete Slide

### Font / Text
- Font Family, Font Size
- Bold, Italic, Underline
- Text Color (theme-first)
- Clear Formatting

### Paragraph
- Bullets, Numbering
- Align Left/Center/Right
- Line Spacing

### Quick Insert
- Text Box
- Image

### AI (Front-and-Center)
- **AI: Rewrite Text…** *(selection/slide; proposal-first)*
- **AI: Tighten to Fit…** *(fix overflow; proposal-first)*
- **AI: Improve Visual Hierarchy…** *(titles/subtitles/body sizing; proposal-first)*
- **AI: Generate Speaker Notes…** *(slide/section; proposal-first)*

---

### 5.4 PPTX — INSERT Tab (REQUIRED)
### Content
- Image
- Shapes
- Table *(V1: simple)*
- Chart *(V1: minimal set; see Live Link below)*
- Hyperlink

### Live Link (Data → Slide) (REQUIRED NORTH-STAR SURFACE)
- **Live Link: Insert Table View…**
- **Live Link: Insert Chart View…**
- Refresh Link (on selected linked object)
- Open Source (jumps to XLSX/CSV/source artifact)

*(If Live Link not available, fallback is “Insert Static Snapshot (labeled)” — but don’t bury Live Link.)*

### AI Assets (Optional V1, but aligned)
- **AI: Generate Diagram…** *(e.g., funnel, flywheel, timeline)*
- **AI: Generate Icons…** *(style-matched set)*

---

### 5.5 PPTX — DESIGN Tab (V1-lean)
- Theme (token-based)
- Variants (light/dark)
- Slide Size
- Background (solid/gradient/image)
- Layouts (apply layout templates)

*(Master editing can exist behind a flag; V1 focus is templates + tokens.)*

---

### 5.6 PPTX — ARRANGE Tab (Keep lean; Inspector is primary)
- Bring Forward / Send Backward
- Align (left/center/right/top/middle/bottom)
- Distribute (H/V)
- Group / Ungroup
- Rotate

---

### 5.7 PPTX — REVIEW Tab (Collaboration + AI) (REQUIRED)
### Comments
- New Comment
- Resolve / Reopen
- Next / Previous Comment

### AI: Narrative + QA (Front-and-center)
- **AI: Consistency Check…** *(fonts, sizes, spacing, tokens)*
- **AI: Layout Fix…** *(align/distribute/overflow; proposal-first)*
- **AI: Fact/Number Cross-check…** *(against project sources; cites file paths)*
- **AI: Executive Summary Slide…** *(build from deck; proposal-first)*
- **AI: Build Appendix…** *(extra charts/tables; proposal-first)*

### Export Readiness
- Accessibility / Contrast Check *(optional V1)*
- “Client-Ready Checklist” *(AI-generated, but deterministic checks where possible)*

---

### 5.8 PPTX — VIEW Tab (REQUIRED)
- Normal
- Grid/Guides/Rulers toggles
- Notes toggle
- Zoom % / Fit to Slide / Fit to Width
- Present (full-screen) *(V1: basic present mode; no rehearsal tooling unless implemented)*

---

### 5.9 PPTX — Inspector (Right Panel) (REQUIRED)
**Selection-aware sections (collapsible):**
1. **AI Actions (contextual)**  
   - Rewrite / Tighten-to-fit / Generate notes / Layout fix (scoped)
2. **Position** *(primary polish surface)*  
   - X/Y, Align, Distribute, Nudge step, Layer order
3. **Size**  
   - W/H, Lock ratio
4. **Typography**  
   - Font, size, spacing, bullets
5. **Fill / Stroke (Theme-first)**  
   - Fill, outline, opacity, token picker
6. **Arrange**  
   - Group, rotate, order
7. **Slide** *(when slide background selected)*  
   - Layout, background, theme variant
8. **Live Link** *(when linked view selected)*  
   - Source, query summary, As-of timestamp, Refresh, Open Source

---

## 6) XLSX (Spreadsheet) — Ribbon + Inspector (V1)

### 6.1 XLSX Tabs (REQUIRED)
**File · Home · Insert · Data · Formulas · Review · View**

---

### 6.2 XLSX — FILE Tab (REQUIRED)
**File Actions**
- Save, Save As…, Save a Copy…
- Export: XLSX, CSV (sheet/range), PDF *(optional V1)*
- Workbook Info: named ranges count, calc mode, last refresh timestamp

**AI (Front Door)**
- **AI: Generate Model…** *(multi-tab workbook; proposal-first)*
- **AI: Add Tabs from Brief…** *(new sheets; proposal-first)*
- **AI: Build from Dataset…** *(ingest + clean + model; proposal-first)*

---

### 6.3 XLSX — HOME Tab (REQUIRED)
### Clipboard
- Paste, Cut, Copy, Format Painter

### Font
- Font family/size
- Bold/Italic/Underline
- Text color, Fill color (theme-first)

### Alignment
- Left/Center/Right
- Top/Middle/Bottom
- Wrap text
- Merge cells *(V1: caution; proposal-first if it impacts ranges)*

### Number
- General / Number / Currency / Percent
- Increase/Decrease decimals

### Cells
- Insert/Delete Row/Column
- Format Cells (dialog)

### Editing
- AutoFill / Fill Down / Fill Right
- Clear (contents/formats)

### AI (Front-and-Center)
- **AI: Write Formula…** *(selection; proposes formula + explanation)*
- **AI: Clean Data…** *(trim, split, types; proposal-first)*
- **AI: Summarize Sheet…** *(structure + key drivers)*

---

### 6.4 XLSX — INSERT Tab (V1-lean)
- Table *(simple)*
- Chart *(minimal set)*
- Hyperlink
- Text box / shapes *(optional; low priority in spreadsheets)*

**AI Insert (aligned)**
- **AI: Create Chart for Selection…** *(proposal-first)*
- **AI: Create “KPI Strip” Sheet…** *(top metrics dashboard)*

---

### 6.5 XLSX — DATA Tab (REQUIRED)
### Get Data / Import
- Import CSV
- Import XLSX (as new sheets)
- Refresh Data *(if connected)*

### Sort & Filter
- Filter toggle
- Sort A→Z / Z→A
- Custom sort

### Data Tools
- Text to Columns
- Remove Duplicates
- Data Validation

### Live Link Publisher (Data → Slide) (REQUIRED NORTH-STAR SURFACE)
- **Publish Selection as Live Link View…** *(creates a named view/query spec)*
- Manage Published Views… *(list + rename + scope)*

### AI (High Leverage)
- **AI: Detect Errors…** *(broken refs, suspicious hardcodes, inconsistencies)*
- **AI: Create Scenario Table…** *(inputs/outputs; proposal-first)*

---

### 6.6 XLSX — FORMULAS Tab (REQUIRED)
- Insert Function (fx)
- Function categories (Lookup, Math, Text, Date, Logical)
- Named Ranges manager
- Trace Precedents / Dependents
- Show Formulas
- Calculation settings *(optional V1)*

**AI Formula (front-and-center here too)**
- **AI: Explain This Formula…**
- **AI: Refactor Formula…** *(simplify / reduce volatility; proposal-first)*

---

### 6.7 XLSX — REVIEW Tab (Collaboration + AI) (REQUIRED)
### Comments
- New Comment
- Resolve / Reopen
- Next / Previous Comment

### AI: Model QA + Insight
- **AI: Model Summary…** *(assumptions, drivers, outputs)*
- **AI: Sensitivity Analysis…** *(one-way/two-way; proposal-first)*
- **AI: Build Charts + Narratives for Deck…** *(creates PPTX slides or slide-ready linked views; proposal-first)*

---

### 6.8 XLSX — VIEW Tab (REQUIRED)
- Freeze Panes
- Show/Hide: Formula Bar, Headings, Gridlines
- Zoom %
- Split view *(optional V1)*
- Toggle Left/Right panels (Explorer/Inspector)

---

### 6.9 XLSX — Inspector (Right Panel) (REQUIRED)
**Selection-aware sections (collapsible):**
1. **AI Actions (contextual)**  
   - Write formula / Clean data / Explain / Error scan
2. **Cell / Range Formatting**  
   - Number format, font, alignment, borders, fill, wrap
3. **Data Validation** *(when applicable)*  
   - allowed values, error message
4. **Conditional Formatting** *(V1 optional; stub ok)*  
   - rules list + quick presets
5. **Table / Range Tools** *(when table/selection)*  
   - header row toggle, totals row
6. **Chart** *(when chart selected)*  
   - type, series, axes, labels
7. **Live Link Publish / Link Info**  
   - publish selection; view name; consumers (slides); as-of timestamp

---

## 7) DOCX (Document) — Ribbon + Inspector (V1)

### 7.1 DOCX Tabs (REQUIRED)
**File · Home · Insert · Layout · References · Review · View**

---

### 7.2 DOCX — FILE Tab (REQUIRED)
**File Actions**
- Save, Save As…, Save a Copy…
- Export: PDF, DOCX
- Print
- Document Info: word count, last export timestamp

**AI (Front Door)**
- **AI: Generate Doc…** *(memo/PRD/brief; proposal-first)*
- **AI: Generate from Sources…** *(project docs → draft; cites sources by file path)*
- **AI: Create Outline…** *(then expand; proposal-first)*

---

### 7.3 DOCX — HOME Tab (REQUIRED)
### Clipboard
- Paste, Cut, Copy, Format Painter

### Styles (constrained, V1)
- Style dropdown (Normal, H1, H2, H3, Bullet, Numbered, Quote)
- Clear formatting

### Font
- Font family/size
- Bold/Italic/Underline
- Text color (theme-first)

### Paragraph
- Align L/C/R
- Bullets/Numbering
- Line spacing
- Indent +/- *(basic)*

### AI (Front-and-Center)
- **AI: Rewrite for Clarity…** *(selection; proposal-first)*
- **AI: Tighten / Shorten…**
- **AI: Expand with Evidence…** *(pulls from project sources; citations via file paths)*
- **AI: Summarize Section…**

---

### 7.4 DOCX — INSERT Tab (REQUIRED)
- Table *(V1 simple)*
- Image
- Hyperlink
- Header/Footer
- Page Number
- Footnote *(can also live in References; keep one consistent entry)*

---

### 7.5 DOCX - LAYOUT Tab (REQUIRED)
- Margins
- Show Margins *(toggle margin guides)*
- Orientation
- Page Size
- Columns *(optional V1)*
- Page Breaks

---

### 7.6 DOCX — REFERENCES Tab (V1-lean, but real)
- Table of Contents *(generate from headings)*
- Footnotes / Endnotes
- Cross-reference *(optional V1)*
- Citations *(V1: lightweight; can be “insert placeholder + manage list”)*

---

### 7.7 DOCX — REVIEW Tab (Collaboration + AI) (REQUIRED)
### Comments
- New Comment
- Resolve / Reopen
- Next / Previous Comment

### AI: QA + Risk
- **AI: Consistency Check…** *(terminology, definitions, tone, headings)*
- **AI: Fact Check Against Project Sources…** *(cites file paths; flags unknowns)*
- **AI: Risk / Gaps Scan…** *(what’s missing for decision-quality)*
- **AI: Turn Comments into Edits…** *(proposal-first)*

*(Track changes can be Phase 2; don’t pretend it exists unless implemented.)*

---

### 7.8 DOCX — VIEW Tab (REQUIRED)
- Print Layout / Reading Layout *(one is enough in V1)*
- Zoom %
- Navigation Pane (outline)
- Ruler toggle *(optional V1)*

---

### 7.9 DOCX — Inspector (Right Panel) (REQUIRED)
**Selection-aware sections (collapsible):**
1. **AI Actions (contextual)**  
   - rewrite/shorten/expand-with-evidence/summarize
2. **Styles**  
   - style selector + quick “normalize headings”
3. **Typography**  
   - font, size, emphasis, color
4. **Paragraph**  
   - alignment, spacing, lists, indents
5. **Page**  
   - margins, orientation, breaks
6. **References**  
   - TOC refresh, footnotes list
7. **Outline / Navigation**  
   - headings tree, jump-to section

---

## Appendix A — Command ID Naming (Lintable Convention)
- App menu: `app.file.newProject`, `app.view.zenMode`
- PPTX: `pptx.home.bold`, `pptx.insert.liveLinkChart`
- XLSX: `xlsx.data.removeDuplicates`, `xlsx.ai.writeFormula`
- DOCX: `docx.layout.margins`, `docx.ai.rewriteClarity`
- AI (shared): `ai.deck.generateFromBrief`, `ai.sheet.generateModel`, `ai.doc.generateFromSources`

**Rule:** Every ribbon/inspector/menu item must map to exactly one command id.

---

### Appendix B Command Definition Template (use only for ambiguous commands)
- **Command ID:** `pptx.ai.tightenToFit`
- **Surface:** Home tab + Inspector
- **Scope:** Selection | Slide | Whole file
- **Inputs:** selected text box(es)
- **Outputs:** proposal change set (no silent mutation)
- **Algorithm (ordered):**
  1) Reduce line breaks where possible
  2) Adjust font size down to min X
  3) Expand box within safe margins
  4) If still overflow → propose rewrite variant A/B
- **Success Criteria:** no overflow; preserves meaning; style tokens unchanged unless allowed
- **Failure Mode:** if cannot fit without exceeding constraints → return “needs layout change” and link to `pptx.ai.layoutFix`

---

## Appendix C — Ambiguous Command Definitions (Registry) (V1)

**Purpose:** Eliminate “verify vibes.” Any command that is **AI**, **Live Link**, or **proposal-first / multi-step** MUST have an entry here.

**Registry Rules**
- **Required fields:** Command ID, Surface, Scope, Inputs, Outputs, Success Criteria, Failure Modes
- **Algorithms:** only required when the command could be implemented in materially different ways (multiple “reasonable” interpretations).
- **Proposal-first default:** Any command that changes more than one object, more than one slide/sheet/section, or rewrites content MUST output a **Change Set Card** (no silent mutation).

---

### C1) Proposal / Change-Set Plumbing (Cross-Editor)

#### `agent.propose.changeSet`
- **Surface:** Assistant (primary), Command Palette
- **Scope:** Selection | Slide/Sheet/Section | Whole File | Project
- **Inputs:** a draft artifact or planned edits from agent
- **Outputs:** Change Set Card containing:
  - semantic diff (what changed)
  - visual diff (where applicable)
  - list of impacted objects (IDs)
  - rollback plan (undo group)
- **Success Criteria:** user can inspect changes before applying; changes apply atomically as one undo step
- **Failure Mode:** if visual diff cannot be produced for a changed region → mark “visual unverified” and require manual review

#### `agent.apply.changeSet`
- **Surface:** Assistant
- **Scope:** Change Set Card (single)
- **Inputs:** approved Change Set Card
- **Outputs:** document mutation + undo checkpoint + updated Inspector/Canvas state
- **Success Criteria:** applied state matches preview; undo restores previous state fully
- **Failure Mode:** any apply conflict → abort apply; present conflict list; keep draft proposal intact

---

### C2) AI “Front Door” Generators (Generate to 90%)

#### `ai.deck.generateFromBrief`
- **Surface:** PPTX File tab (primary), Command Palette
- **Scope:** Whole File | Project
- **Inputs:** Brief Card fields:
  1) audience + decision
  2) deck length + slide types (e.g., 10 slides; problem/solution/market…)
  3) sources (project files/links/none)
  4) constraints (brand tokens, tone, definitions)
  5) “done means…” acceptance criteria
- **Outputs:** Change Set Card that includes:
  - slide list (titles + intent)
  - generated slide content (text + layout choices)
  - speaker notes (optional, unless requested)
- **Algorithm (ordered):**
  1) propose outline (slide titles + purpose)
  2) fill slides with draft content
  3) run `pptx.ai.consistencyCheck` (tokens/typography)
  4) run `pptx.ai.layoutFix` (overflows/alignment)
- **Success Criteria:** deck is coherent, readable, no obvious overflow, consistent tokens, meets slide count
- **Failure Mode:** missing sources for factual claims → flag as “needs sources” and insert explicit TODO tags

#### `ai.sheet.generateModel`
- **Surface:** XLSX File tab (primary), Command Palette
- **Scope:** Whole File | Project
- **Inputs:** Brief Card with:
  - model type (budget, metrics model, unit economics, scenario model)
  - tab count + tab roles (Inputs, Calc, Outputs, Charts)
  - definitions (units, currency, time period)
  - sources/datasets (optional)
- **Outputs:** Change Set Card containing:
  - created sheets + named input ranges
  - formulas in calc areas
  - outputs table + at least one chart if requested
- **Algorithm (ordered):**
  1) create tabs: Inputs / Calc / Outputs / Charts (unless overridden)
  2) define input block + named ranges
  3) build calc + checks (balance tests, error flags)
  4) build outputs + charts
  5) run `xlsx.ai.detectErrors`
- **Success Criteria:** model runs without broken refs; inputs clearly separated; outputs interpretable
- **Failure Mode:** if requirements ambiguous → return a “Model Spec Clarification” proposal instead of guessing

#### `ai.doc.generateFromSources`
- **Surface:** DOCX File tab (primary), Command Palette
- **Scope:** Whole File | Project
- **Inputs:** document type (memo/PRD/brief), length target, tone, required sections, sources
- **Outputs:** Change Set Card containing:
  - outline (H1/H2)
  - drafted content
  - citations as **file path references** (not external links unless provided)
- **Algorithm (ordered):**
  1) propose outline
  2) draft sections with explicit “unknowns” callouts
  3) run `docx.ai.consistencyCheck`
  4) run `docx.ai.riskGapsScan`
- **Success Criteria:** structured doc with headings; claims traceable to provided sources or labeled unknown
- **Failure Mode:** insufficient sources → keep draft but add “Evidence Needed” section

---

### C3) PPTX AI Actions (Ambiguous by Nature)

#### `pptx.ai.tightenToFit`
- **Surface:** PPTX Home tab + Inspector + Command Palette
- **Scope:** Selection | Slide
- **Inputs:** one or more text boxes
- **Outputs:** Change Set Card (no silent mutation)
- **Algorithm (ordered):**
  1) reduce line breaks where possible
  2) adjust font size down to **min_font_size_token** (default: Theme minimum)
  3) expand box within safe margins (respect slide padding + layout bounds)
  4) if still overflow → propose rewrite variant A/B (same meaning, shorter)
- **Success Criteria:** no overflow; meaning preserved; style tokens unchanged unless explicitly allowed
- **Failure Mode:** if cannot fit without violating constraints → return “needs layout change” and link to `pptx.ai.layoutFix`

#### `pptx.ai.layoutFix`
- **Surface:** PPTX Review tab (primary) + Inspector
- **Scope:** Slide | Selection | Whole File
- **Inputs:** slide(s) or selection
- **Outputs:** Change Set Card with annotated before/after
- **Algorithm (ordered):**
  1) detect overflow + misalignment + inconsistent spacing
  2) apply align/distribute + consistent margins
  3) resolve overlaps by reflow rules (never hide content)
  4) if conflicts remain → propose “split slide” option
- **Success Criteria:** fewer overlaps/overflows; consistent spacing; no content loss
- **Failure Mode:** if slide density too high → propose “split into 2 slides” instead of compressing illegibly

#### `pptx.ai.consistencyCheck`
- **Surface:** PPTX Review tab
- **Scope:** Whole File | Section | Slide
- **Inputs:** current deck + theme tokens
- **Outputs:** Findings panel + optional Change Set Card to auto-fix
- **Success Criteria:** identifies inconsistent fonts/sizes/colors/margins; provides fixes mapped to tokens
- **Failure Mode:** if tokens missing → generate a “Token Proposal” instead of random fixes

#### `pptx.ai.factNumberCrossCheck`
- **Surface:** PPTX Review tab
- **Scope:** Slide | Whole File | Project
- **Inputs:** deck text + project sources
- **Outputs:** Findings list with:
  - claim
  - source file path reference (or “no source found”)
  - confidence tag (Verified/Inferred/Unknown)
- **Success Criteria:** flags unsupported claims; links to sources
- **Failure Mode:** if no sources exist → return “cannot verify” without fabricating

---

### C4) XLSX AI Actions (Ambiguous by Nature)

#### `xlsx.ai.writeFormula`
- **Surface:** XLSX Home tab (primary) + Inspector + Command Palette
- **Scope:** Selection (target cell or range)
- **Inputs:** natural language intent + selected target(s)
- **Outputs:** Proposal (Change Set Card) that includes:
  - formula inserted into target cell(s)
  - plain-English explanation
  - example with sample inputs (if safe)
- **Algorithm (ordered):**
  1) infer intended output per cell
  2) propose formula(s) using consistent references
  3) validate for #REF / range mismatch / obvious errors
  4) propose alternative formula if volatile/complex
- **Success Criteria:** correct references; explanation matches; no broken refs
- **Failure Mode:** ambiguous intent → ask via a “Clarification Card” (single question max) instead of guessing

#### `xlsx.ai.cleanData`
- **Surface:** XLSX Home tab + Data tab
- **Scope:** Selection | Sheet
- **Inputs:** selected range + desired target (in-place vs new sheet)
- **Outputs:** Change Set Card showing before/after samples
- **Success Criteria:** transformations are explicit (trim/split/type-cast); no silent destructive edits
- **Failure Mode:** if destructive (loses information) → must default to “create cleaned copy” mode

#### `xlsx.ai.detectErrors`
- **Surface:** XLSX Data tab (primary) + Review tab
- **Scope:** Sheet | Whole File
- **Inputs:** workbook
- **Outputs:** Findings list + optional Change Set Card to fix:
  - broken references
  - inconsistent units
  - suspicious hardcodes in calc blocks
  - circular refs (if supported)
- **Success Criteria:** flags issues with cell addresses and explanations
- **Failure Mode:** if cannot evaluate (calc disabled) → mark as “calc unavailable” and stop

---

### C5) DOCX AI Actions (Ambiguous by Nature)

#### `docx.ai.rewriteClarity`
- **Surface:** DOCX Home tab + Inspector
- **Scope:** Selection | Section
- **Inputs:** selected text + target tone (default: “direct, professional”)
- **Outputs:** Change Set Card with:
  - rewritten text
  - “what changed” summary (bullets)
- **Success Criteria:** preserves meaning; improves clarity; avoids adding new claims
- **Failure Mode:** if rewriting introduces new facts → flag and reject that rewrite variant

#### `docx.ai.expandWithEvidence`
- **Surface:** DOCX Home tab + Review tab
- **Scope:** Selection | Section | Whole File | Project
- **Inputs:** text + project sources
- **Outputs:** Change Set Card with:
  - expanded prose
  - inline citations as file path references
  - explicit “unknowns” where evidence missing
- **Success Criteria:** all added claims are sourced or labeled unknown
- **Failure Mode:** if no evidence found → propose questions + “Evidence Needed” list instead of inventing

#### `docx.ai.riskGapsScan`
- **Surface:** DOCX Review tab
- **Scope:** Whole File
- **Inputs:** doc + acceptance criteria (“done means…”)
- **Outputs:** structured list:
  - missing sections
  - unclear assumptions
  - decision blockers
  - suggested appendices
- **Success Criteria:** actionable gaps, not generic advice
- **Failure Mode:** if doc type unknown → first propose a doc type classification

---

### C6) Live Link Commands (Cross-Editor)

#### `xlsx.data.publishLiveLinkView`
- **Surface:** XLSX Data tab + Inspector
- **Scope:** Selection (range/table/chart data)
- **Inputs:** selected range + view name + refresh policy (manual by default)
- **Outputs:** a persisted **Live Link View Spec** containing:
  - source file id + sheet + range / query
  - schema snapshot (column names/types)
  - view name + stable view id
- **Success Criteria:** view can be re-resolved after minor sheet edits; consumers listed
- **Failure Mode:** if selection is unstable (no headers, ragged range) → require user confirmation or convert to table first

#### `pptx.insert.liveLinkTableView`
- **Surface:** PPTX Insert tab + Inspector
- **Scope:** Slide (insert object)
- **Inputs:** chosen Live Link View Spec + display options (show headers, max rows)
- **Outputs:** slide object that renders the view + stores:
  - view id
  - last refresh timestamp (“as-of”)
  - visual style tokens
- **Success Criteria:** refresh updates content without breaking layout; open source navigates correctly
- **Failure Mode:** if refresh would overflow layout → propose `pptx.ai.tightenToFit` or “paginate table” option

#### `pptx.insert.liveLinkChartView`
- **Surface:** PPTX Insert tab
- **Scope:** Slide (insert object)
- **Inputs:** Live Link View Spec + chart type + series mapping
- **Outputs:** linked chart object + as-of timestamp + source mapping
- **Success Criteria:** updates preserve mapping; no silent type switching
- **Failure Mode:** if schema changes → show “mapping broken” banner with repair wizard

#### `livelink.refreshSelected`
- **Surface:** Inspector (when linked object selected), context menu
- **Scope:** Selected object
- **Inputs:** none (uses stored view id)
- **Outputs:** updated rendering + updated as-of timestamp
- **Success Criteria:** deterministic refresh; errors reported with cause
- **Failure Mode:** unreachable source → keep last good render; display stale badge

---

## Appendix D — Save / Export / Import Semantics (Non-Destructive I/O) (V1)

**Goal:** Prevent silent overwrites and make fidelity testable.

### D1) Save vs Export (Cross-Editor)

**V1 Truth:** Save writes the **active editable artifact** in the Project:
- PPTX editor → saves `.pptx`
- XLSX editor → saves `.xlsx`
- DOCX editor → saves `.docx`
- Text editor → saves `.md/.txt/.json/.yaml`

Imported Office files become the editable Project copy. Nexus does not modify the original external source path.

**Export** generates a **new derived output** (Office/PDF/images) without silently overwriting:
- Default: new file name (or timestamped) + chosen folder.
- Overwrite: allowed only with explicit user confirmation (“Overwrite existing file?”).

**Phase 3+ (North Star):** Save may additionally persist a Nexus-native IR artifact (`.nxsdeck/.nxssheet/.nxsdoc`) as a governed, diffable source—without changing the V1 semantics above unless explicitly enabled.

### D2) Import Sidecars (Fidelity Hook)
- Every import MUST produce an Import Fidelity Ledger under `.nexus/` (ignored by git), keyed by `relativePath`:
  - Example: `.nexus/ledgers/import/<relativePath>.audit.json`
- UI MUST provide a one-click path to open the ledger:
  - Program Menu → Project → Fidelity Reports
  - Or Editor FILE tab → “Fidelity Report…”

### D3) Export Sidecars (Fidelity Hook)
- Every export MUST produce an Export Fidelity Ledger under `.nexus/` (ignored by git), keyed by `relativePath`:
  - Example: `.nexus/ledgers/export/<relativePath>.audit.json`
- UI MUST provide a one-click path to open the export audit summary:
  - Editor FILE tab → “Export Report…”

---

## Appendix E — Automation & Verification Contract (Agents + CI) (V1)

**Purpose:** Make visual + functional verification deterministic.

### E1) Stable Selectors
- Every command surface (menu item, ribbon button, inspector control, context menu entry) MUST have:
  - `data-testid` = **Command ID** (exact string)
- Ribbon scaffolding MUST have stable ids:
  - Tabs: `ribbon.tab.file`, `ribbon.tab.home`, `ribbon.tab.insert`, etc.
  - Groups: `ribbon.group.clipboard`, `ribbon.group.ai`, etc. (type-specific allowed)

### E2) Deterministic Apply Semantics
- Any command defined as proposal-first MUST route through:
  - `agent.propose.changeSet` → Change Set Card → `agent.apply.changeSet`
- Apply MUST be atomic and create **one undo checkpoint**.

### E3) Visual Verification Outputs (Minimum)
For any agent-run workflow that changes rendering:
- Capture `before.png` and `after.png` (same viewport + zoom)
- Persist a `semantic_diff.json` summary:
  - bbox changes, overflow flags, z-order changes, token changes
- If semantic diff cannot be computed, mark the change set as **visual unverified**.

### E4) Test-Only Debug Seam (Verification Hook)
- **Requirement:** On any successful command dispatch, the Shell must set:
  ```javascript
  window.__NEXUS_LAST_COMMAND = { id: string, surface: string, engineId: string, ts: number };
  ```
- **Rule:** E2E Tests MUST assert against this global to verify dispatch, rather than relying on "vibe-based" side effects.

### E5) Canonical Command IDs (Hardened List)
- **Outline Toggle:** `id: "docx.view.toggleOutline"`, `data-testid: "docx.view.toggleOutline"` (or legacy `nexus-doc-toggle-outline` allowed if aliased).
- **Show Margins Toggle:** `id: "docx.view.toggleMargins"`, `data-testid: "docx.view.toggleMargins"`.


---

## Appendix F — Honest UI Banners (Fidelity + Unsupported Features) (V1)

**Rule:** If fidelity ledgers indicate loss/approximation, the UI must say so.

### F1) Import Warnings
- If Import Ledger includes any `DROPPED` items:
  - Show a persistent banner: “This file contains unsupported features.”
  - Provide “View Fidelity Report” action.
- If Import Ledger includes `APPROXIMATED` items:
  - Show a warning banner: “Some features were approximated.”

### F2) Export Warnings
- If Export Ledger includes any `DROPPED` or `APPROXIMATED`:
  - Show an export summary banner with “View Export Report.”

### F3) Black Box Preservation Disclosure
- If the file contains preserved raw blobs (unsupported elements preserved for round-trip):
  - Show an info badge: “Unsupported elements preserved for round-trip.”
  - Provide “View Preserved Elements” action (read-only list).
