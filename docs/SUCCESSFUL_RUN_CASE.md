# Acceptance Design: A Removed Bullet Must Stay Removed

**Illustrative acceptance standard · No verification result is claimed on this page**

## Start with the durable user outcome

If a user removes a bullet from a document paragraph, the saved document should reopen with that paragraph still plain. Where nested children remain, their text, order, and depths should remain intact.

A marker disappearing in the current window is insufficient evidence of a durable edit.

## Require evidence that can challenge the claim

A useful acceptance check would inspect both the saved representation and the application state after a full quit/reopen. Building and judging the result should be separate roles.

A negative control asks whether the check would detect removal of the repair. A check that still passes when the relevant repair is absent cannot support that repair's claim.

These are acceptance-design principles, not reported results for a particular private build.

## Why this matters to knowledge work

The same discipline applies to a financial model, a recommendation, or an AI-produced document: define the actual outcome, inspect the result, challenge an attractive conclusion, and state what the evidence cannot establish.

My role is to define that operating practice and make its acceptance standards explicit.

## Public evidence boundary

This page publishes no private verification results, logs, source identifiers, or internal report excerpts. The [recorded interaction failure](AUDITABLE_RUN_CASE.md) and [synthetic business work sample](BUSINESS_WORKFLOW_CASE.md) provide separate public examples with their own provenance and limits.

[Overview](../README.md) · [Workflow](WORKFLOW.md) · [Current status](CURRENT_STATUS.md)
