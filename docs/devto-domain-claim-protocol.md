---
title: "Introducing the Domain Claim Protocol: Contracts for Agentic Systems"
published: false
description: "Why agentic systems need explicit claims about capabilities, boundaries, dependencies, and faults before components can be safely composed."
tags: ai, agents, architecture, opensource
---

Software is moving from fixed call chains toward systems that choose tools, services, and other agents as they work. That flexibility is useful, but it makes a basic engineering question harder to answer: **What is each component actually allowed to do?**

An API can tell an agent how to call a function. A JSON schema can tell it what fields to send. Neither necessarily tells it who owns a business decision, which provider it depends on, what it refuses to do, or how a failure should affect the rest of the workflow.

As a workflow grows, those missing agreements become *agent spaghetti*: implicit boundaries, drifting context, and failures that surface far from their cause. We need a way to make the agreements explicit before components are assembled.

That is the idea behind the [Domain Claim Protocol (DCP)](https://github.com/UnfurlSystemsLab/dcp), an open draft specification from Unfurl Systems Lab. The current public draft is **`0.2.0-draft`**.

## The agent composition bottleneck

Consider an agent asked to reconcile ledger entries against a bank feed. It may be able to discover a `reconcile` endpoint and format the request correctly. That still leaves questions an endpoint description may not answer:

- Is the feed verified, and who decides that?
- Which ledger data does this component own?
- What happens when the verification provider is unavailable?
- Can this component approve a transaction, or must it refuse that decision?
- Which failure should stop the parent workflow and alert a person?

Prompt instructions can describe some of these constraints, but the assembly also needs contracts that a compiler, runtime, and human reviewer can inspect consistently.

## What a domain claim describes

A DCP **claim** is a component's structured description of its role. It identifies the component and states its domain, capabilities, required providers, explicit refusals, conflict rules, integration ports, and declared faults. A claim makes the component's boundaries available to machines and people before execution.

Here is an **illustrative excerpt**, not a complete valid claim:

```yaml
identity:
  uri: dcp://components/ledger-reconciler
  name: Ledger Reconciler
  kind: component
  version: 0.1.0
  publisher: Example Organization

domain:
  summary: Reconciles ledger entries against verified bank feeds.
  concerns:
    - concern: ledger-reconciliation
      description: Compares ledger entries with verified feed entries.
  boundary_principles:
    - Reconciliation requires a verified bank feed.

refusals:
  - concern: transaction-approval
    rationale: Approval belongs to an authorized decision maker.

offers:
  - capability: ledger.reconcile
    description: Reports reconciliation differences for a verified feed.
    consumer_access: named_components_only
    interface:
      kind: in_process
      details:
        operation: reconcile
    stability: experimental
    version: 0.1.0

metadata:
  dcp_version: 0.2.0-draft
  claim_version: 0.1.0
```

`dcp_version` identifies the protocol draft; `claim_version` and the offer's `version` identify this example component and capability. Those are separate version axes. The omitted sections matter: a full claim also describes dependencies, conflicts, and faults, and it must satisfy the [current schema](https://github.com/UnfurlSystemsLab/dcp/tree/master/docs/schemas). The repository has a [component example](https://github.com/UnfurlSystemsLab/dcp/blob/master/docs/examples/simple-component-claim.yaml) and [aggregate example](https://github.com/UnfurlSystemsLab/dcp/blob/master/docs/examples/aggregate-dcp.yaml) for exploring the draft shape.

An operational limit is only useful when a system knows where to enforce it. DCP can declare a boundary or constraint; the accepted contract, runtime binding, and host policy determine how that declaration applies in a particular assembly. Likewise, a declared fault gives a failure a machine-readable meaning and an allowed path for propagation or remediation. It does not make a runtime automatically recover from every error.

## From claim to execution

DCP separates the workflow into three planes:

| Plane | Question | Artifact |
| --- | --- | --- |
| Description | What does this component claim about itself? | Claim |
| Negotiation | Can these components compose under the relevant context and policy? | Frozen composition contract |
| Invocation | Is this call allowed under the accepted contract? | Deterministic runtime result |

The important boundary is between negotiation and invocation. The system can reason about whether components fit together at design time, then execute against the resulting contract without asking a model to renegotiate every call.

A **runtime binding** supplies environment-specific wiring for that contract, such as provider endpoints and references to configuration or secrets. An aggregate can reference child claims, contracts, and bindings so reviewers can inspect what sits behind a higher-level capability.

## The Unfurl ecosystem

DCP is the protocol layer in a larger, modular effort:

- [**DCP specification**](https://github.com/UnfurlSystemsLab/dcp) defines the public draft, schemas, vocabulary, and examples.
- [**unfurl-dcp**](https://github.com/UnfurlSystemsLab/unfurl-dcp) implements the protocol as a Java library.
- [**unfurl-fabric**](https://github.com/UnfurlSystemsLab/unfurl-fabric) handles design-time negotiation and compilation of governed compositions.
- [**unfurl-ui**](https://github.com/UnfurlSystemsLab/unfurl-ui) is the emerging visual workspace, or *human harness*, for inspecting and questioning assemblies before they run.

The human harness is an intended review surface, not a claim that every proposed workflow is already safe. Its value depends on the quality of the claims, validation, policy, and runtime enforcement beneath it.

## Join the review

DCP is a **draft specification** open to correction through real examples. We are especially interested in cases where a tool can be called correctly but still should not be used: unclear ownership, conflicting authority, hidden dependencies, cost limits, or faults that need to stop a larger workflow.

Read the [specification and examples](https://github.com/UnfurlSystemsLab/dcp), then bring a concrete case to [GitHub Discussions](https://github.com/UnfurlSystemsLab/dcp/discussions) or [open an issue](https://github.com/UnfurlSystemsLab/dcp/issues). The public specification is licensed under [Apache 2.0](https://github.com/UnfurlSystemsLab/dcp/blob/master/LICENSE).
