# DCP Faults Specification

**Status:** Draft.
**Plane:** Description and invocation.
**Schema:** `docs/schemas/fault.schema.yaml`.
**Source material:** `docs/HLD-C-dcp-v0.2-internal.md`, section 3.8, and
`docs/HLD-C2-dcp-schema-spec-updated.md`, section A.10.

## 1. Purpose

DCP faults are declared protocol signals. They explain what can fail, what
evidence justifies the fault, what claim surface is affected, and whether the
fault propagates to a parent aggregate.

Faults are not untyped logs. They are part of the claim boundary.

## 2. Fault Declaration

A claim MUST include a `faults` section. It MAY be empty when the component has
no declared DCP-visible fault vocabulary.

Each declared fault SHOULD include:

- stable fault code;
- description;
- severity;
- affected needs, offers, constraints, or ports;
- evidence signals;
- propagation policy;
- remediation actions when allowed.

## 3. Propagation Gate

A child fault propagates upward only when the declared fault affects a
parent-visible need, dependency, constraint, offer, or service expectation.

The propagation decision MUST be deterministic:

```text
declared fault vocabulary + observed signal -> propagate | suppress | reject
```

The propagation gate MUST NOT perform design-time negotiation.

## 4. Invalidation

If a fault invalidates a frozen contract assumption, runtime MUST hard-fail the
affected call and emit an invalidation signal. Fabric or another design-time
tool MAY renegotiate later.

## 5. Change Impact

This document makes faults a dedicated spec while preserving the current DCP
v0.2 behavior.
