# Nexus: AI Workflow Design in Practice

**AI workflow design · Agentic orchestration · Evidence-led knowledge work**

I’m JB Monu. I design workflows that turn AI capacity into useful, reviewable work. Nexus is my private R&D project: a local-first desktop workspace for business strategy, with native presentation, document, and spreadsheet editors.

I own product direction, workflow design, acceptance standards, and final judgment. AI coding agents perform most implementation work. My contribution is connecting the problem, the agents, the evidence, and the decision.

![Nexus displaying a synthetic business decision with an imported contribution chart](assets/nexus-northstar-decision.jpg)

*Real Nexus window capture, October 1, 2026. Fictional company and synthetic data. The downloadable presentation was authored with AI assistance outside Nexus, then imported and displayed in its native editor. [Demo provenance and limits](docs/BUSINESS_WORKFLOW_CASE.md).*

## What I bring to a team

| Capability | Evidence in this portfolio |
| --- | --- |
| Turn an ambiguous goal into accountable work | Bounded outcomes, explicit exclusions, and acceptance standards in the [operating workflow](docs/WORKFLOW.md) |
| Design agentic orchestration | A primary orchestrator, a bounded build orchestrator, and independent proof roles in an [implemented execution graph](docs/EXECUTION_GRAPH.md) |
| Challenge apparently successful AI output | A [successful verification case](docs/SUCCESSFUL_RUN_CASE.md) with a deletion control, alongside a [recorded interaction failure](docs/AUDITABLE_RUN_CASE.md) |
| Connect analysis to a business decision | A [synthetic strategy case](docs/BUSINESS_WORKFLOW_CASE.md) linking an editable model, decision memo, and presentation |
| Keep claims proportional to evidence | Dated [status and known limitations](docs/CURRENT_STATUS.md), including issues found during this showcase |

These are relevant to strategy, operations, program management, and AI adoption work: defining the right question, coordinating execution, reviewing evidence, and communicating a decision. Nexus is a concrete example of that practice.

## From prompting to orchestrating

After I approve an outcome, Claude Code’s primary orchestrator owns routine routing, sequencing, setup, verification, and integration. A bounded build orchestrator handles implementation and retries within the locked outcome. When a harness boundary requires it, I relay one brief and one return; product and scope decisions remain mine.

The graph is more than a diagram. Its private implementation defines states, role ownership, required evidence, and repair paths. The verification acceptance path checks named agent identities and evidence against the dispatched work. This is **execution-graph design and agentic orchestration**, with AI agents implementing much of the code.

The gate writer and verifier are each different agents from the builder. A passing report does not authorize the builder to approve itself.

~~~mermaid
flowchart LR
    A[Human approves outcome] --> B[Scope and proof contract]
    B --> C[Independent gate]
    C --> D[Bounded build]
    D --> E[Independent verification]
    E -->|Evidence supports claim| F[Integration and completion proof]
    E -->|Failure or contradiction| B
    F --> G[Record outcome and review learning]
~~~

*Simplified development workflow. It is separate from the still-incomplete graph and assistant capabilities inside the Nexus product. Routine process autonomy does not mean unattended success on every item.*

[Inspect the graph design and its evidence boundaries](docs/EXECUTION_GRAPH.md).

## A stronger view of the product

The Northstar example asks whether a fictional field-services business should expand immediately or run a five-site pilot. The model makes an $80,000 monthly cost sensitivity visible; the memo explains what the model cannot establish.

![Nexus displaying the synthetic decision memo](assets/nexus-northstar-memo.jpg)

*Real document-editor capture, October 1, 2026. The memo was authored with AI assistance, then imported into Nexus. This is a constructed work sample, with no claimed client engagement or business impact.*

[See the scenario slide, workbook view, assumptions, and downloadable files](docs/BUSINESS_WORKFLOW_CASE.md).

## Evidence that the loop can catch the real failure

In an October 1 verification report, an independent agent checked whether removing a DOCX host bullet survived native save and a full quit/reopen. The scoped gates and a 57-case parity bank passed. Deleting the reader repair made the relevant persistence checks fail again.

That provides a stronger example than a green badge: the check reacted to removing the fix. The public case preserves the narrow scope, the export refusal, and the distinction between a private report and a publicly reproducible test.

[Read the scoped successful run](docs/SUCCESSFUL_RUN_CASE.md).

## Project status

Nexus remains private, unshipped R&D with no production customers. Office export, editing coverage, preservation, and fidelity remain incomplete. The screenshots show particular imported files in a local prototype; they do not establish complete Office compatibility.

## Explore

- [Case study: my role and transferable practice](docs/CASE_STUDY.md)
- [Workflow: authority, handoffs, and review](docs/WORKFLOW.md)
- [Execution graph: implemented controls and boundaries](docs/EXECUTION_GRAPH.md)
- [Business work sample: model → memo → presentation](docs/BUSINESS_WORKFLOW_CASE.md)
- [Successful run](docs/SUCCESSFUL_RUN_CASE.md) · [Failure case](docs/AUDITABLE_RUN_CASE.md) · [Current status](docs/CURRENT_STATUS.md)

This repository contains sanitized portfolio material, not the private Nexus source. Documentation and showcase artifacts were drafted and revised with LLM assistance; I remain responsible for what I publish.

[LinkedIn](https://www.linkedin.com/in/jb-monu-9a58543) · [GitHub](https://github.com/jbmonu87)
