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
It does not introduce new fields beyond the current DCP v0.2 draft.
