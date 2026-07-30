# DCP Aggregation Specification

**Status:** Draft.
**Plane:** Description, negotiation, and runtime binding.
**Source material:** dynamic composite model in `docs/HLD-C-dcp-v0.2-internal.md`
and recursive projection notes in `docs/HLD-C2-dcp-schema-spec-updated.md`.

## 1. Purpose

DCP aggregation is the protocol construct for parent/child DCP composition. A
parent DCP can reference child claims, child contracts, and child runtime
bindings while exposing a higher-level capability surface.

Aggregation is not a UI-only grouping. It is a governed protocol relationship.

## 2. Child References

Parent artifacts MAY reference child artifacts:

- claim -> child claims;
- contract -> child contracts;
- runtime binding -> child runtime bindings.

References MUST preserve child inspectability. A parent MUST NOT hide child
contracts when a parent-visible capability depends on them.

## 3. Projection

Projection walks the DCP reference graph and produces a deterministic view of
depth, parent, descendants, offers, needs, and edges. Projection MUST guard
against cycles and runaway depth.

## 4. Parent Capability Surface

A parent capability is valid only when it is supported by accepted child
capabilities, parent-owned capabilities, or both. Parent policy decides which
child details are exposed or abstracted, but the underlying references remain
available for inspection.

## 5. Anti-Pattern

Do not introduce product-specific sidecar blocks to store child composition
state. The referenced DCP construct is the standard aggregation pattern.

## 6. Change Impact

This document records aggregation as a first-class DCP spec. It does not add a
new schema field beyond existing child/reference concepts.
