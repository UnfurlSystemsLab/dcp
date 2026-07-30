# DCP Composition Contract Specification

**Status:** Draft.
**Plane:** Negotiation output.
**Schema:** `docs/schemas/contract.schema.yaml`.
**Source material:** `docs/HLD-C2-dcp-schema-spec-updated.md`, section B.

## 1. Purpose

A composition contract is the frozen artifact produced by Plane 2 negotiation.
It records which claims participated, which needs were bound to which offers,
which conflicts were resolved, what runtime binding is expected, and what
provenance/trust context applies.

The contract is the cache between negotiation and invocation.

## 2. Contract Rules

A contract MUST:

- identify itself with a stable `contract_id`;
- name its DCP `contract_version`;
- list participating claims;
- record accepted bindings from consumer needs to provider offers;
- record rejected or unresolved needs;
- include policy and trust context sufficient for verification;
- include or reference child contracts when the contract is an aggregate.

A contract MUST NOT rely on hidden local state to explain why a need was bound
or rejected.

## 3. Frozen Behavior

Runtime invocation MUST execute against the frozen contract. If an invocation
needs a binding that the contract did not accept, the call MUST fail rather than
renegotiate in the hot path.

## 4. Child Contracts

A parent contract MAY reference child contracts. Parent-visible capabilities are
the deterministic projection of accepted child capabilities and parent policy.

## 5. Verification

Verification checks the contract structure, participating claim references,
accepted bindings, trust/provenance records, and signatures where present.

## 6. Change Impact

This document extracts the contract specification from the current schema source
without changing contract semantics.
