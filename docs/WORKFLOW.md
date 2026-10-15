# Working With AI Agents: The Nexus Loop

The Nexus workflow is designed around one principle: **the person accountable for the outcome must be able to inspect why the work should be trusted.**

My contribution is designing the division of work and the standard for accepting it. The agents carry out most implementation. For a real example of review changing a deliverable, [follow the showcase run](RUN_WALKTHROUGH.md); for actual editor behavior, start with [product evidence](PRODUCT_EVIDENCE.md).

## The loop

| Stage | Question | Output |
| --- | --- | --- |
| Frame the work | What outcome matters, and what is the smallest useful item? | Scope, owner, and an isolated workspace |
| Define success | What must be true, and how will we know? | Acceptance criteria, required evidence, and reasons to stop |
| Make failure observable | Can a different agent show the defect before implementation starts? | An independent failing check when existing proof is inadequate |
| Build | Can the bounded change satisfy the gate without changing it? | Implementation, targeted checks, challenge, and health evidence |
| Verify | Does a different agent reproduce the claim through the real path? | Pass, fail, or hold with evidence |
| Integrate and prove | Is the verified item safe to integrate, and does the actual result work? | Integration plus machine or human completion proof |
| Record and learn | What should the next run know that this one did not? | A completion record and, when justified, a reusable check |

## Two orchestrators, two levels of authority

Nexus currently uses two orchestration layers. The public diagram names roles rather than model brands because model assignments change more often than the operating design.

**The primary orchestrator runs the program.** After I approve a work item, it selects the lane, sets up a separate workspace, routes the roles, and carries the item through verification and closeout. It escalates product behavior, scope, and irreversible decisions to me.

**The build orchestrator runs one bounded build.** It can choose a builder, call for a critique or narrow research, make limited retries, and return the evidence. It cannot change the product goal or the proof it must meet, approve the result, or merge it.

When the two orchestration environments cannot hand work directly to each other, I act as a mechanical relay for one outbound brief and one inbound evidence package. I do not make the routing or retry decisions in between.

## Research before committing to a diagnosis

When the cause is uncertain, research precedes implementation. Observed facts, inferences, and unknowns stay separate. An implementation hypothesis may change when evidence contradicts it; the intended user outcome cannot quietly become easier to satisfy.

## Role boundaries

| Role | Owns | Does not own |
| --- | --- | --- |
| Human product owner | Intended outcome, product behavior, scope, material risk, and final human judgment | Routine routing, retries, or workspace administration |
| Primary orchestrator | Program state, sequencing, role assignment, proof separation, integration, and closeout | Production implementation or changing product intent |
| Researcher / hardener | Root-cause evidence, failure model, acceptance criteria, required evaluation classes, and stop rules | Building against an untested diagnosis or weakening the outcome |
| Independent gate writer | A check that fails for the intended reason before the builder starts | Production code or making its own test pass |
| Build orchestrator | One bounded build arc, retries, critic, health checks, and one evidence return | Product decisions, gate changes, approval, or merge |
| Builder | The scoped implementation and supporting evidence | Writing or weakening its own gate; approving its own work |
| Independent verifier | Reproduction through the real path and a pass, fail, or hold recommendation | Repairing the result while judging it |

The two proof boundaries are fixed: the gate writer and verifier must each be different agents from the builder. Model-family diversity is useful when it is cheap; agent independence matters more than brand names.

## How the run becomes inspectable

Role-labelled changes and completion records connect the requirement, implementation, independent checks, and acceptance decision. A later human test can reopen an item that looked complete. The [execution graph](EXECUTION_GRAPH.md) explains those relationships and separates this development process from the unfinished connected-work features inside Nexus.

## How a run improves the next run

An observation becomes useful when it changes the next decision or check. For example, the [recorded interaction failure](AUDITABLE_RUN_CASE.md) showed that identifying the correct element did not prove the click could reach it. The lesson was to inspect the actual interaction point. Unproved explanations stay labelled as hypotheses; a passing check does not automatically become a general rule.

## Example of a bounded work packet

### Outcome

When a user selects presentation thumbnail **N**, slide **N** becomes the active slide.

### In scope

- selection from the visible thumbnail list;
- the active-state update; and
- one deterministic check of the selected identifier.

### Out of scope

- thumbnail redesign;
- keyboard navigation;
- multi-selection; and
- unrelated presentation behavior.

### Acceptance criteria

1. Selecting thumbnail 4 activates slide 4.
2. The check fails before the repair and passes afterward.
3. The evidence reads the real active-slide state rather than relying on timing or visual guesswork.
4. A human confirms the interaction in the running product.

### Evidence receipt

A useful completion record answers:

- What changed?
- What exact claim was tested?
- What evidence was produced?
- What was already failing before the work began?
- What remains incomplete or uncertain?
- Who performed the final review?

## Four failure patterns the workflow is meant to catch

1. **The moving finish line:** a failing check is made easier instead of the product being repaired.
2. **The inherited failure:** an existing problem is incorrectly blamed on the new work because no baseline was recorded.
3. **The green proxy:** an automated check passes, but it never observed the result the user sees.
4. **The combined judge and builder:** the same agent implements, interprets ambiguity in its favor, and declares success.

## Where the loop stops

Low-risk work can use fewer roles. Higher-risk work earns more separation and stronger evidence. Automated checks remain fallible, and human experts remain responsible for consequential decisions.

[Return to the overview](../README.md) · [Read the case study](CASE_STUDY.md) · [Read an acceptance-design example](SUCCESSFUL_RUN_CASE.md) · [Read a failure case](AUDITABLE_RUN_CASE.md) · [See current status](CURRENT_STATUS.md)
