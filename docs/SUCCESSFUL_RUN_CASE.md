# A Scoped Success: Removing a Bullet Must Survive Reopen

**Verification report dated October 1, 2026 · Sanitized private evidence summary**

## The user outcome

A user removes the bullet from an ordinary DOCX body paragraph that has nested children. After native save, another layout change, another save, and a full application quit/reopen, the host must remain plain. The children's text, order, and depths must remain intact.

A visually removed marker in the current session is inadequate evidence if the saved document later reopens as a bullet.

## Reported independent results

The reviewed verifier report records:

| Check | Completed result |
| --- | --- |
| State and export-contract checks, H1/H2 | 2 of 2 passed |
| Real Electron persistence gate, H3 | 1 of 1 passed; Backspace and Shift+Tab subjects, two saves, full quit/reopen |
| Corrected parity bank | 57 of 57 passed across three display-scale settings |
| Delete the reader repair | H1 and the selected G11 cold-reopen check failed for the intended persistence defect |
| Restore the reader | Byte-identical restoration; subsequent real-app probes used the restored build |

The deletion control left the saved representation and tests intact but removed the reader's repair. The saved host then projected back into the wrong bullet state. The check rejected the product defect rather than merely failing to compile or launch.

## A useful complication

The parity oracle needed correction to read each paragraph's own saved numbering before counting retained ancestors. A separate gate-writing role made that correction. The verifier reviewed it and independently demonstrated that the corrected check still detected removal of the reader repair.

This matters because a pass after editing a test deserves scrutiny. Here the report describes the preserved sequence/depth assertions and the negative control that challenges the corrected oracle.

## Additional real-app probes

The report describes two fresh synthetic subjects, each saved and reopened in a fresh Electron process:

- Remove the host bullet, type in the host, and preserve three descendant levels.
- Remove the host bullet, then restore its original bullet with the real ribbon control before saving.

The first probe's strict DOCX export visibly refused the unsupported list edit and produced no output. The second restored the original representation and exported source parts that the verifier's ZIP comparison found byte-identical. Neither result establishes general list-edit export support.

## Why this belongs in the portfolio

It connects product judgment to evidence design: specify the durable user outcome, separate building from verification, inspect the real file and cold app state, and ask whether the test would still pass with the repair removed.

My contribution is setting and developing that operating practice. The implementation and verification work were carried out by AI coding agents.

## Limits and traceability

The source report is private: this public summary was checked against it, but the tests were not rerun for the portfolio update. Readers cannot reproduce the full private test bank from this repository.

Private evidence anchors reviewed: candidate 4e04217739505ea5ff7e306698950be951204ce4; builder commits d14f1834a and 0d2dffaa2; verifier report in docs/specs/DocxHostBulletOutdentPersists/verify_1001.md. These are identifiers for traceability, not public source links.

The report's verdict is VERIFIED-SCOPED, not whole-product completion. Table-cell outdent, arbitrary mixed split/merge operations, broad numbering styles, Office pixel parity, and broad OOXML list export remain unclaimed. Initial H3 screenshots were overwritten by a later bank run; its completed log and separately preserved cold-probe captures remain.

[Overview](../README.md) · [Earlier failure case](AUDITABLE_RUN_CASE.md) · [Current status](CURRENT_STATUS.md)
