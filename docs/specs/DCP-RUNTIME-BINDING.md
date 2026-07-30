# DCP Runtime Binding Specification

**Status:** Draft.
**Plane:** Deployment/runtime projection.
**Schema:** `docs/schemas/runtime-binding.schema.yaml`.
**Source material:** `docs/HLD-C2-dcp-schema-spec-updated.md`, section C.

## 1. Purpose

A runtime binding maps a frozen composition contract to concrete runtime
resources for a target environment. It is mutable environment wiring, not a new
composition decision.

Runtime binding answers: where is the provider, what transport is used, which
secret/config references resolve it, and which runtime ports are bound?

## 2. Binding Rules

A runtime binding MUST reference a composition contract. It MUST NOT introduce a
provider or capability that the contract did not accept.

Runtime bindings MAY vary by tenant, environment, deployment target, or release
stage.

## 3. Secret And Config References

Secrets and environment-specific values MUST be referenced using secret/config
references. Raw credentials MUST NOT appear in a DCP runtime binding.

## 4. Child Bindings

An aggregate runtime binding MAY reference child runtime bindings. This is the
standard way to bind a parent DCP contract that depends on child DCP contracts.

## 5. Validation

Validation MUST confirm that every provider reference, port, secret reference,
and config reference required for execution is present and belongs to the
accepted contract surface.

## 6. Change Impact

This document extracts the runtime binding specification from the current schema
source without changing binding semantics.
