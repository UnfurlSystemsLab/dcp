# DCP Webapp Manifest Specification

**Status:** Draft.
**Plane:** Description projection.
**Schema:** `docs/schemas/webapp-manifest.schema.yaml`.
**Source material:** `docs/HLD-C-dcp-v0.2-internal.md`, section 4, and
`docs/HLD-C2-dcp-schema-spec-updated.md`, section D.

## 1. Purpose

The webapp manifest is the frontend/host-shell projection of a DCP claim. It
allows a host shell to understand how a component may be presented, opened,
configured, and governed without treating UI metadata as a separate source of
truth.

## 2. Source Of Truth

The claim is the source of truth. A webapp manifest MUST be generated from or
validated against the same component description as the claim.

## 3. Projection Boundary

The manifest MAY include frontend and host-shell fields such as display name,
entry points, navigation hints, permissions, and hosted-in-shell behavior. It
MUST NOT override domain ownership, refusals, dependencies, offers, faults, or
runtime binding requirements declared by the claim.

## 4. Change Impact

This document extracts the webapp manifest projection into its own spec page. It
does not change the current v0.2 draft field model.
