# DCP HTTP Binding Specification

**Status:** Draft.
**Plane:** Invocation (transport binding).
**Schemas:** `docs/schemas/http-invocation.schema.yaml`, `docs/schemas/registration-report.schema.yaml`.
**Source material:** [`unfurl-dcp` HLD-C2](https://github.com/UnfurlSystemsLab/unfurl-dcp/blob/master/docs/HLD-C2-dcp-schema-spec-updated.md), sections C and G.3; `docs/HLD-C-dcp-v0.2-internal.md` §6.

## 1. Purpose

`transport.kind: http_json` maps a contract invocation to an HTTP request with a
JSON body. This page defines that mapping, the paths a DCP HTTP host serves, and
the registration report through which a host states what it has registered.

## 2. Service Root

A provider reached over HTTP has a runtime binding of kind `container`,
`remote_service` or `sidecar`. Its endpoint MUST be exactly
`<service_root>/dcp/<provider_capability>`. The service root is that endpoint
with the suffix removed. It is never configured separately.

The runtime binding is the only source of a service root.

## 3. Invoke

`POST <service_root>/dcp/<capability>` with a JSON body:

- `invocation` (required): `contractId`, `operation`, `consumerComponent`,
  `providerComponent`, `input`, `correlationId`, `traceContext`,
  `integrityHash`, `metadata`;
- `context` (optional): `tenantId`, `userId`, `roles`, `permissions`,
  `correlationId`, `requestId`, `traceContext`, `metadata`.

These are the deployed wire names. They are an explicit exception to DCP's
snake_case convention; renaming them is a breaking wire change.

`invocation.operation` selects which of the offer's documented operations
applies. `input` must match that operation's request schema, and a successful
`output` its response schema. A failed result (`success: false`) carries an
`errorCode`, and its `output` is any object (normally `{}`) that is never
checked against the response schema. Both `input` and `output` are JSON objects.

A capability that reached the provider answers `200` with `success`, `output`,
`errorCode`, `errorMessage` and `metadata`. Capability faults are `200` with
`success: false` and a declared fault code. `400` means a malformed envelope,
`404` a capability that is not registered on this host, and `503` a host that is
not ready.

`X-Correlation-Id` and W3C `traceparent` headers are propagated when present.

## 4. Registration Report

`GET <service_root>/dcp/registrations` returns:

- the host's component identity and version;
- the canonical hash of its own packaged claim;
- the time of the report;
- every capability registered right now: contract id and version, capability,
  surface, path, and the provider's claim URI, version and hash;
- the product API surfaces it serves.

The provider fields are per-registration evidence. For a capability the host
serves itself, they repeat the host's own identity. For a component it hosts in
process, they are that component's identity and the claim hash the host verified
against the contract's pin when it admitted it. A host never reports a hosted
component under its own identity.

Every DCP HTTP host MUST serve it. It lists contracts only: no inputs, outputs,
credentials, secret references or caller data. A revoked or invalidated contract
is absent. The response carries `Cache-Control: no-store`.

Enumeration values are lower case (`surface: http | hosted_in_process`).

The capability name `registrations` is reserved, so the report never collides
with an invoke path. A claim that offers a capability with that name is invalid.
Every host's OpenAPI document describes this route (DCP Documentation §4A).

## 5. Identity

Identity follows the signed composition, never configuration.

- **Identity contract present.** Invocation requires `Authorization: Bearer`,
  and a body `context` is rejected with `400`; the host derives context from the
  verified token. `401` means a missing or invalid token and `403` an
  insufficient permission. The registration report requires permission
  `dcp.registrations.read`; callers use the OAuth 2.0 client-credentials grant.
- **No identity contract.** There is no bearer requirement, a body `context` is
  accepted, and the registration report is unauthenticated. The host runs on a
  trusted network and warns at startup.

## 6. Observation Limits

A caller that observes registration reports (for example a documentation
projector) probes only service roots derived from runtime bindings. It never
scans networks or ports, never resolves unrelated DNS names, and never reads
orchestrator APIs. Each root is probed once per interval, with a timeout no
larger than the binding's `runtime_policy.timeout_ms`.

## 7. Aggregation

Each child binding names its own provider's endpoint. A host that serves
capabilities for several children has one service root and one report listing
all of them.

## 8. Change Impact

This page is new. It specifies the `http_json` transport that HLD-C §6 names,
the endpoint shape the runtime binding must use, and the registration report.
