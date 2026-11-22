# DCP Claim Specification

**Status:** Draft.
**Plane:** Description.
**Schema:** `docs/schemas/claim.schema.yaml`.
**Source material:** `docs/HLD-C2-dcp-schema-spec-updated.md`, section A.

## 1. Purpose

A DCP claim is a component's self-description. It states what the component owns,
what it refuses, what it needs, what it offers, what faults it can produce, and
what metadata and integration ports describe its boundary.

A claim is static and versioned. It is the source of truth for component
composition.

## 2. Required Top-Level Elements

A claim MUST contain:

- `identity`
- `domain`
- `refusals`
- `dependencies`
- `offers`
- `conflict_resolution`
- `faults`
- `metadata`

A claim MAY contain:

- `negotiation_surface`
- `integration_ports`
- `child_claims`
- `api_documents`

If `identity.kind == intelligent_component`, `negotiation_surface` MUST be
present.

## 3. Domain Boundary

The claim boundary is carried by `domain.concerns`, `domain.boundary_principles`,
and `refusals`. A claim with no specific refusals is not a valid DCP boundary.

State and decisions belong under the concern that owns them. DCP does not define
parallel top-level `state_owned` or `decisions_owned` fields.

## 4. Needs And Offers

`dependencies` define what the component needs. `offers` define what the
component exposes for others to consume. Plane 2 negotiation binds a consumer
need to a provider offer when compatibility, policy, version, and binding rules
allow it.

Offer versions MUST use semantic versioning. Consumer requirements use semantic
version ranges.

An offer's capability MUST NOT be `registrations`. The DCP HTTP binding
reserves that name for `GET <service_root>/dcp/registrations` (DCP HTTP Binding
§4).

An offer MAY carry `documentation`: a non-empty list of `operations`, one per
`invocation.operation` value the offer accepts. Each operation has a
`request_schema` for its `input` and a `response_schema` for its successful
`output`, each an `artifact_ref` whose root is `type: object`. Operations are
selected by the envelope's `operation`, never inside a payload schema.
Documentation is not matched by the resolver and never changes which needs an
offer satisfies. Payload schemas MUST be stated through `documentation`;
free-form hints in `interface.details` are not documentation proof.

## 4A. Shipped Artifacts

An `artifact_ref` names one file inside the artifact that carries the claim:
a relative `path`, a `media_type` (JSON Schema or OpenAPI), and the file's
`sha256`. The reference is valid only with its hash. A reader MUST verify the
hash before use, and a missing file or a mismatch is a validation failure, never
a fallback. JSON Schemas use draft 2020-12, OpenAPI documents use 3.1.0, and both
are self-contained:

- a JSON Schema's `$ref`s are JSON-Pointer fragments into the same file;
- an OpenAPI document's `$ref`s target its own `#/components/...`;
- `$id`, `$anchor` and the dynamic-reference keywords are not allowed.

`api_documents` ship OpenAPI descriptions of the component's product HTTP
routes. Each operation names the served surface it belongs to with
`x-dcp-surface`, and its access with `x-dcp-access: public | protected`
(required, no default). It may name a permission with `x-dcp-permission`. A
document declares no servers, no security and no `/dcp/` paths: those come from
the runtime binding, the identity contract and the offers. Two documents of one
component may not declare the same method and path. API documents describe
routes only; they never create contracts.

## 5. Ports

`integration_ports` names common enterprise integration concerns such as
authentication, authorization, telemetry, monitoring, secrets/configuration, and
AI/model access. Ports describe dependencies and expectations; they do not
implement those concerns.

## 6. Child Claims

A claim MAY reference child claims. Child references are the standard DCP
aggregation construct. Sidecar extension blocks MUST NOT replace child DCP
references for aggregate composition.

## 7. Validation

Validation MUST reject a claim that is missing required sections, has no
specific refusal, has no domain concern, or uses an invalid identity/version
shape. Validation SHOULD warn when the claim is technically valid but too vague
to support reliable composition.

## 8. Change Impact

This document separates the claim spec from the larger schema source document.
It adds three optional, additive fields to the DCP v0.2 draft:
`offers[].documentation`, `api_documents`, and the `artifact_ref` value type
(HLD-C2 A.6, A.12, A.13). A claim that does not use them keeps its canonical
hash, because absent fields are not serialized.
