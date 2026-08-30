# Case Study: Turning AI Activity Into Accountable Work

## The challenge

AI can produce an impressive amount of work quickly. That creates a management problem as much as a technical opportunity: speed is valuable only when someone can define the outcome, recognize failure, and decide what evidence is trustworthy.

I created Nexus as a private learning lab for that problem. It is an unshipped Office-style prototype with presentation, document, and spreadsheet workflows. I chose this domain because the work is deceptively difficult. A file may load and a test may pass while a user still sees the wrong slide, broken formatting, or a change that does not survive save and reopen.

That makes Nexus a useful place to learn how AI-assisted work should be scoped, checked, and governed.

## My role

I lead Nexus as its creator and product owner. AI coding agents perform most implementation work; I do not present their output as code I personally authored.

I am responsible for:

- deciding what problem to solve and why it matters;
- translating ambiguous goals into small work packets;
- defining the user-visible outcome and acceptance criteria;
- directing research, build, and verification agents;
- deciding what evidence is strong enough to support a claim;
- reviewing failures, changing the workflow, and making scope tradeoffs; and
- performing the final human smoke test.

Their speed makes the management and verification system more important, not less.

## The key operating decision

I separated three kinds of work that AI systems often blur together:

1. **Research and test design:** identify the real risk and define how failure will be observed.
2. **Build:** make the bounded change without changing the success criteria.
3. **Verification:** independently decide whether the evidence supports the claim.

The builder does not get to declare its own work correct. A passing automated check can support a decision, but it does not replace judgment about whether the check observed the real user outcome.

## A representative failure

In one class of Nexus work, automated evidence looked green while the visible result remained wrong. The check had observed a convenient proxy, not the behavior the user actually experienced.

The valuable output was not merely a repaired feature. It was a stronger operating rule:

- define the visible outcome before implementation;
- confirm that the test can actually observe that outcome;
- prevent the builder from weakening the test to obtain a pass;
- compare against known baseline failures before attributing a new failure; and
- require a human smoke test when visual or interactive behavior matters.

This is the distinction at the center of the project: **a result can be technically green and still be operationally untrustworthy.**

## What I learned

### Tight scope improves both speed and judgment

“Improve the editor” is not an executable assignment. “When a user selects item N, item N becomes active and remains active after save and reopen” is bounded, observable, and reviewable.

### Independent verification changes agent behavior

When build and verification are separate, there is less incentive to reinterpret a requirement around the work already produced. The verifier evaluates the claim rather than defending the implementation.

### Baselines prevent false blame

Complex systems often begin with unrelated known failures. Recording the starting state makes it possible to distinguish a new regression from existing debt.

### Proof must match the decision

A unit test can prove an internal rule. It cannot always prove a rendered layout, an interaction, or a durable saved file. The evidence must be appropriate to the claim.

### The human remains accountable

Agents can research, build, critique, and test. A person still owns the intended outcome, the acceptable risk, and the final decision.

## How this transfers beyond software

The same pattern applies to business work involving research, analysis, documents, decisions, or operating processes:

- define the decision or user outcome;
- split broad work into bounded assignments;
- specify evidence before execution;
- separate production from review where the risk warrants it;
- test the real deliverable, not a convenient proxy; and
- record failures as improvements to the process.

That is the capability I am building through Nexus: not “prompting,” and not a claim to be an engineer, but the ability to turn AI capacity into useful, reviewable, human-owned work.

## Claim boundary

Nexus is private, unshipped R&D. The current prototype is incomplete and has no production customers. This case study describes my product ownership and AI-workflow practice; it does not claim commercial traction, production readiness, or authorship of all implementation code.

[Return to the overview](../README.md) · [See the workflow](WORKFLOW.md) · [See current status](CURRENT_STATUS.md)
