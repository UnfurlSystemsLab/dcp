# DCP Negotiation Specification

**Status:** Draft.
**Plane:** Negotiation.
**Source material:** `docs/HLD-C-dcp-v0.2-internal.md`, section 5, and
`docs/HLD-C2-dcp-schema-spec-updated.md`, sections E and F.

## 1. Purpose

DCP negotiation determines whether claims can compose under a given context,
policy, and action. Its output is a frozen composition contract.

Negotiation may use human judgment, deterministic rules, model assistance, or a
combination, but invocation MUST NOT repeat negotiation.

## 2. Negotiation Function

At minimum, negotiation evaluates:

- consumer needs;
- provider offers;
- dependency satisfaction;
- conflicts and refusals;
- required ports and bindings;
- fault and invalidation behavior;
- trust/provenance policy;
- child/referenced DCP closure.

## 3. Question Schema

The question schema is the structured form of a negotiation question. It exists
so the same issue can be rendered to a human operator or model-assisted
authoring tool without changing semantics.

Questions SHOULD identify:

- the artifact and field being questioned;
- the blocked decision;
- available choices or expected answer shape;
- consequences of each answer;
- whether the question blocks contract generation.

## 4. Action Context

Design-time action context narrows questions to the current authoring/editing
intent. For example, adding a component should ask different questions than
removing a provider or rebinding a runtime port.

## 5. Change Impact

This document separates negotiation from the broader core spec. It preserves the
current v0.2 rule that negotiation is design-time only.
