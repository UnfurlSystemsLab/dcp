# DCP Faults Specification

**Status:** Draft.
**Plane:** Description and invocation.
**Schema:** `docs/schemas/fault.schema.yaml`.
**Source material:** `docs/HLD-C-dcp-v0.2-internal.md`, section 3.8, and
[`unfurl-dcp` HLD-C2](https://github.com/UnfurlSystemsLab/unfurl-dcp/blob/master/docs/HLD-C2-dcp-schema-spec-updated.md), section A.10.

## 1. Purpose

DCP faults are declared protocol signals. They explain what can fail, what
evidence justifies the fault, what claim surface is affected, and whether the
fault propagates to a parent aggregate.

Faults are not untyped logs. They are part of the claim boundary.

## 2. Fault Declaration

A claim MUST include `faults: { emitted: [...] }`. The list MAY be empty when the
component has no declared DCP-visible fault vocabulary. Each entry follows
[`fault.schema.yaml`](../schemas/fault.schema.yaml), which is HLD-C2 A.10:

- `code`: a stable machine code, unique within the claim;
- `category`: `DEPENDENCY`, `CAPABILITY`, `CONSTRAINT`, `HEALTH`, `SECURITY`,
  `POLICY` or `RUNTIME`;
- `severity`: `INFO`, `WARNING`, `DEGRADED`, `CRITICAL` or `BLOCKING`;
- `description`;
- `affects`: the declared `needs`, `offers` and `constraints` the fault affects,
  at least one of them non-empty;
- `evidence.signals`: `HEALTH`, `INVOCATION_ERROR`, `METRIC_THRESHOLD`,
  `POLICY_DENIAL`, `CONTRACT_INVALIDATION` or `EXTERNAL_EVENT`;
- `propagation`: `parent_impact` (`NONE`, `DEGRADED` or `BLOCKED`), with
  `propagates_when` required unless it is `NONE`, and an optional
  `suppresses_when`;
- `remediation.allowed_actions`: only actions the claim boundary or host policy
  already permits.

Enum values are written upper case on the wire; field names are snake_case.

## 3. Propagation Gate

A child fault escalates to its parent aggregate only as its declared
`propagation` says: `parent_impact` is the impact on the parent (`DEGRADED` or
`BLOCKED`; `NONE` never escalates), `propagates_when` is the condition a runtime
or parent resolver evaluates before escalating, and `suppresses_when` is the
condition under which it does not.

The propagation decision MUST be deterministic:

```text
(claim.faults, fault_signal) -> propagate | suppress | reject
```

An observed signal for an undeclared fault is rejected. The propagation gate
MUST NOT perform design-time negotiation, and never renegotiates a contract in
the runtime path.

## 4. Invalidation

If a fault invalidates a frozen contract assumption, runtime MUST hard-fail the
affected call and emit an invalidation signal. Fabric or another design-time
tool MAY renegotiate later.

## 5. Change Impact

This document makes faults a dedicated spec while preserving the current DCP
v0.2 behavior.
