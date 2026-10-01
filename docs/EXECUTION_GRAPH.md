# Execution-Graph Design and Agentic Orchestration

**Source inspection: October 1, 2026. Development workflow, separate from the Nexus product graph.**

## What I designed

The workflow treats AI work as a sequence of owned states with evidence requirements. I define the operating model and acceptance standards; AI coding agents implement much of the control-plane code.

The primary orchestrator can decide the routine next action after human approval. A bounded build orchestrator can manage implementation, critique, setup repair, and limited retries. It cannot redefine the goal, weaken the gate, approve its own result, or merge it.

When direct transport across harnesses is unavailable, a human may relay one outbound brief and one return. That is a remaining transport dependency.

## What is implemented in the private source

The inspected state-machine definition is version 9. It includes:

- explicit state ownership and permitted transitions;
- required dispatch, commit, agent-identity, and evidence fields;
- separate paths for setup failures, gate repair, product failures, lost attempts, and re-verification;
- verification, integration, retirement, and completion as distinct stages.

A representative transition has this shape:

~~~text
VERIFY_RUNNING
  -- VERIFY_ACCEPT by independent-verifier -->
VERIFY_ACCEPTED

Required fields include:
VerifiedSha, BuilderAgentId, VerifierAgentId,
IntegrationContract, EvidencePath, EvidenceDigest
~~~

This is a simplified transcription of the inspected contract, not executable public source.

The verification acceptance implementation checks that:

1. the result binds the dispatched tree, branch, and envelope;
2. builder and verifier identities match their evidence and differ from each other;
3. required command IDs and definitions match the dispatch, with collection floors;
4. the integration contract matches the verifier-owned evidence and digest.

The source also checks the expected parents of an integration merge and a separately bound integration receipt. The model distinguishes a merge from a checked integration and from final completion.

## Why it matters

An agent saying “done” is not the same event as independent verification accepting a named change. Keeping those stages separate gives the orchestrator a precise next action and gives the human a basis for review.

The useful skill here is designing the states, authority, recovery routes, and evidence requirements around the work. “Execution-graph design” and “agentic orchestration” describe that contribution. This portfolio does not assert professional software-engineering credentials or personal authorship of all implementation code.

## Evidence boundary

This page summarizes private source inspection, not a fresh runtime test of every transition. It does not prove that every historical run used version 9, that identity labels alone establish independence, or that all guard bypasses are impossible.

The [successful run case](SUCCESSFUL_RUN_CASE.md) supplies a separate, narrow example of reported independent verification. The private source and complete operating logs are not published here, so external readers cannot reproduce the entire control plane from this repository.

The development execution graph is implemented. The envisioned Nexus product graph linking business workspaces, routines, artifacts, views, and decisions remains incomplete. The screenshots do not demonstrate that full in-app system.

[Overview](../README.md) · [Operating workflow](WORKFLOW.md)
