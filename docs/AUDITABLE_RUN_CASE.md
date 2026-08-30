# One Auditable Run: When the Correct Target Was Still Unclickable

This is a sanitized account of a real Nexus verification run. Internal paths, identifiers, logs, and source code remain private.

## The work item

Verify a real presentation-editing control before using it in a broader functionality review.

The acceptance standard was user-visible: the automated interaction had to reach the intended slide element, change the expected behavior, and leave inspectable evidence. Finding an element in the document structure was necessary but not sufficient.

## Role separation

| Role | Responsibility in this run |
| --- | --- |
| Product owner / orchestrator | Set the acceptance standard, scope, and stop rule |
| Test designer | Select the real control and define the observable outcome |
| Builder | Maintain the bounded implementation under test |
| Verifier | Run the interaction, inspect the evidence, and classify the result independently |
| Human reviewer | Decide whether the evidence justified continuing the larger review |

## What happened

The source-scoped selector identified the intended slide text element. The automated click then timed out.

It would have been easy to treat that as test instability, retry it, or choose an easier target. Instead, the run inspected the actual element at the intended click point. A transform handle was covering the target's center, so the click did not reach the text element.

The selector was correct. The interaction was still broken.

## The decision

The result was classified as a product failure. It did not count as a pass or an infrastructure problem. The larger review stopped at that preflight rather than generating a misleading set of downstream results.

This mattered because continuing would have produced activity and counts without a trustworthy interaction foundation.

## The reusable change

The failure became a stronger operating rule:

1. Prove the target comes from the intended source and active document.
2. Separately prove that the planned interaction point is usable.
3. When a click fails, inspect the topmost element at that point instead of retrying blindly.
4. Treat a failed preflight as terminal evidence for the broader run.
5. Do not turn a product failure into a passing result by weakening the target or assertion.

## What the graph preserved

The useful output was not only the bug diagnosis. The work item connected:

- an explicit user-visible claim;
- a source-scoped target;
- a verifier-run observation;
- a failed disposition;
- a stop decision; and
- a new rule for future work.

That chain is what I mean by a graph-shaped AI operating system: decisions and evidence retain their relationships, so the next run can start from a better standard rather than from a vague memory of what happened.

## Claim boundary

This case demonstrates one real failure-handling pattern. It does not establish that every Nexus workflow follows the model perfectly, that the underlying product defect has been repaired, or that Nexus is production-ready.

[Return to the overview](../README.md) · [See the workflow](WORKFLOW.md) · [See current status](CURRENT_STATUS.md)
