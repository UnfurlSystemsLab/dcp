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

## 3A. Provider Endpoints

A provider reached over the DCP HTTP binding (`container`, `remote_service` or
`sidecar`) has an endpoint, given as `base_url` or `base_url_ref`, of exactly
`<service_root>/dcp/<provider_capability>`. An `in_process` provider has no
endpoint and is reached through its host. `external_saas` and `webapp`
providers are not DCP-HTTP invocable, and neither is a provider whose offer is
`external_protocol`, whatever its deployment kind: its binding has no
`base_url`, and the protocol's endpoints are configuration values and
references. A binding that needs an endpoint and has
none is incomplete. Documentation projection uses the binding as its only
endpoint source. See [DCP HTTP Binding](DCP-HTTP-BINDING.md).

## 4. Child Bindings

An aggregate runtime binding MAY reference child runtime bindings. This is the
standard way to bind a parent DCP contract that depends on child DCP contracts.

## 5. Validation

Validation MUST confirm that every provider reference, port, secret reference,
and config reference required for execution is present and belongs to the
accepted contract surface.

## 6. Change Impact

This document extracts the runtime binding specification from the current schema
source. Section 3A adds the endpoint-shape rule for HTTP-served providers
(HLD-C2 C, G.3); bindings that already use that shape are unchanged.
