# Current Status and Claim Boundaries

**Evidence reviewed October 15, 2026 · Private, unshipped R&D · No production customers**

## How to read the evidence

**Shown functioning** means the stated interaction or view was observed in the application. **Independently machine-verified** means a different AI agent from the builder ran checks against a pinned build; it does not mean an external audit. **Founder-tested** requires a recorded human test of that particular outcome. A screenshot alone establishes neither persistence nor acceptance.

This update inspected the private integration source, dated verification records, retained captures, and integration history. Both October 15 changes below are present in the integration branch. Their runtime evidence comes from the separately rebuilt verification candidates before integration, not a new full-app run of the latest integration build. No private source or raw engineering records are published here.

## What the evidence supports

| Capability or observation | Evidence | Acceptance and limits |
| --- | --- | --- |
| Presentation, document, and spreadsheet inputs displayed in native editors | **Shown functioning:** October 1 Northstar window captures | Particular synthetic files and views. The existing dist build’s source commit was not established. Broad editing and fidelity are not proved. |
| Spreadsheet title edit and displayed values retained after native save and tab reopen | **Shown functioning:** earlier October 1 capture iteration | No full application restart was tested in that observation. Applied currency/percentage formats did not survive. |
| Ordinary document-text insert/delete/cut/paste recording; Undo/Redo and targeted Accept/Reject; native save/full reopen; supported inline DOCX revision export | **Independently machine-verified October 15:** 10/10 primary feature cases, companion checks, and six additional keyboard probes; real application capture retained | Integrated October 15; post-integration typecheck and 176 focused Python checks passed. Founder typing/markup acceptance and actual exported-file viewing in Word remain pending. |
| Sampled spreadsheet values, formulas, formatting, sheet structure, and pending cell entry retained across workbook switches and targeted save/full reopen | **Independently machine-verified October 15:** 21/21 active feature cases, separate disk checks, and full-reopen probes; retained app captures | Integrated October 15; typecheck passed. A post-integration runtime recheck and founder acceptance are still owed. This is scoped preservation evidence, not universal spreadsheet correctness. |
| Northstar review changed an incorrect financial label and the public artifacts | **Recorded review and revision:** October 1–2 public showcase history | A bounded knowledge-work cycle, separate from the complete product-development workflow and in-app autonomy. |

No new founder-tested acceptance is claimed for the two October 15 changes. [View their captures and provenance](PRODUCT_EVIDENCE.md).

## What remains partial or failing

- **Document review:** the new evidence concerns ordinary body text and supported inline revision export. Tracking formatting, paragraph marks, moves, or header/footer edits remains outside that delivered slice. A tracked paragraph join persists natively but strict DOCX export refuses the unsupported structural change by name.
- **Spreadsheet preservation:** the October 1 Northstar currency/percentage-format loss and zoom-triggered dirty state have not been specifically rechecked. New passing tests on other fixtures do not close those observations.
- **Broader regression checks:** the document verification sweep retained 299 passes, 16 failures, and 7 skips; spreadsheet verification retained 33 passes, 6 failures, and 1 skip. The records attribute baseline failures and other-item issues rather than presenting either sweep as all green. Spreadsheet review also retains a test-discrimination gap and an intermittent document close/save failure whose cause remains unresolved.
- **Import/render fidelity and responsiveness:** complex files, pagination, precise keyboard behavior, caret feel, and performance still have open work. A passing focused check is not a guarantee across documents or a blanket speed comparison with other editors.
- **General Office export and unsupported-content preservation:** incomplete. Preserving unsupported data remains a product requirement, not an achieved universal guarantee.

The October 1 replacement-deck observation also remains open: importing corrected content under an existing filename initially displayed older slide text on selection. Fresh import filenames were used for the published captures; the cause was not established in that session.

## Planned or not demonstrated

The wider in-app business-work graph, automatic cross-artifact Live Links, and end-to-end autonomous analysis or document generation are not demonstrated by this portfolio. The Northstar model, memo, and deck were authored outside Nexus, then imported; the downloadable files are those inputs, not Nexus exports.

The development workflow used to coordinate coding agents is distinct from the still-incomplete connected-work experience within the product. Visible assistant controls do not prove those capabilities work.

## Authorship and public scope

I own product direction, workflow design, requirements, acceptance standards, and final judgment. AI coding agents perform most implementation work. Routine coordination is delegated within approved outcomes; product decisions and human acceptance remain my responsibility, and some cross-environment handoffs still require a relay.

The public repository contains selected real captures, synthetic inputs, high-level operating descriptions, and bounded examples. It does not establish commercial traction, customer outcomes, enterprise adoption, complete Office compatibility, or personal authorship of the production code. Northstar’s figures are hypothetical.

[Overview](../README.md) · [Product evidence](PRODUCT_EVIDENCE.md) · [Run walkthrough](RUN_WALKTHROUGH.md) · [Work sample](BUSINESS_WORKFLOW_CASE.md)
