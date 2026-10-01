# Execution-Graph Design and Agentic Orchestration

**Operating design · Development workflow, separate from the Nexus product graph**

## What I designed

I use an execution graph to organize AI work around owned stages, bounded authority, evidence, and recovery paths. I define the operating model and acceptance standards; AI coding agents perform most implementation work.

After I approve an outcome, the primary orchestrator handles routine routing and sequencing. A bounded build orchestrator handles implementation and retries within the outcome. It cannot redefine the goal, weaken the acceptance standard, or approve its own result.

When direct transport across harnesses is unavailable, I may relay one brief and one return. That is a remaining transport dependency.

## What the graph makes explicit

| Stage | Responsibility | Required distinction |
| --- | --- | --- |
| Outcome and scope | Human product owner | Product intent versus routine process |
| Acceptance contract | Research and hardening roles | The required outcome versus an implementation hypothesis |
| Gate | Independent gate writer | The checker versus the builder |
| Build | Bounded build orchestrator and builder | Implementation versus approval |
| Verification | Different agent from the builder | A completion claim versus accepted evidence |
| Integration and closeout | Primary orchestrator | A verified change versus an integrated, completed item |
| Recovery | Appropriate role for the failure | Setup failure, contract contradiction, and product failure |

A failed or contradictory result routes to a responsible role. It does not silently redefine success.

## Why it matters

An agent saying “done” is not the same event as independent verification accepting a change. Keeping those stages separate gives the orchestrator a precise next action and gives the human a basis for review.

“Execution-graph design” and “agentic orchestration” describe my work on this operating system. This portfolio does not assert professional software-engineering credentials or personal authorship of all implementation code.

## Evidence boundary

This page describes my operating design. It publishes no private code excerpts, internal contract fields, source-version details, or private verification results. It is not a public executable implementation or proof that every run satisfies every intended safeguard.

The development workflow is distinct from the envisioned Nexus product graph linking business workspaces, routines, artifacts, views, and decisions. That in-app system remains incomplete, and the screenshots do not demonstrate it.

[Overview](../README.md) · [Operating workflow](WORKFLOW.md) · [Acceptance example](SUCCESSFUL_RUN_CASE.md)
