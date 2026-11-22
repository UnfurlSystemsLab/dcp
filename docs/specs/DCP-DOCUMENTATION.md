# DCP Documentation Projection Specification

**Status:** Draft.
**Plane:** Documentation projection.
**Schemas:** `docs/schemas/documentation-projection.schema.yaml`, `docs/schemas/registration-report.schema.yaml`.
**Source material:** `docs/HLD-C2-dcp-schema-spec-updated.md`, section G (and A.6, A.12, A.13).

## 1. Purpose

DCP documentation projection generates runtime-facing documentation from
accepted DCP surfaces only. It prevents documentation from exposing capabilities
that were not accepted by the claim/contract/binding chain.

The projection is generic: it documents any component in a verified composition,
using only DCP artifacts and one standard runtime observation per service. It
needs no product-specific code.

## 2. Projection Rule

Generated documentation MUST be derived from accepted claims, offers, contracts,
and runtime bindings. It MUST NOT expose hidden implementation endpoints or
unaccepted provider capabilities.

A projector's inputs are exactly:

1. the verified composition: the signed aggregate root and its exact child closure;
2. the provider claim of each child, matching the hash the child pins;
3. the files those claims reference through `artifact_ref`, verified by SHA-256;
4. the children's runtime bindings;
5. one registration report per service root (see [DCP HTTP Binding](DCP-HTTP-BINDING.md));
6. whether the verified set contains an identity contract;
7. a product-supplied visibility policy.

Plugin jars, classpath scans, tool or provider registries, model catalogs,
prompts, logs, implementation DTOs, free-form `interface.details` hints,
`metadata.references`, environment variables, and network or orchestrator
discovery are never documentation proof.

## 3. The Documentation Decision

Every child contract is one documentation entry. An entry is documented only
when:

- it is in the verified closure;
- its runtime binding is enabled;
- its endpoint (for HTTP-served providers) has the shape
  `<service_root>/dcp/<capability>`;
- it passes two separate checks against the registration report of its
  service root:
  - the **host check**: the report's component, version and claim hash equal
    the host pinned for that root;
  - the **provider check**: the report lists the contract, and that
    registration's provider claim URI, version and hash equal the entry's pin.
    For a component hosted in process, this is the hash the host verified when
    it admitted it;
- its visibility is `public` or `internal`.

All HTTP entries on one service root must pin the same provider; otherwise
projection fails with `SERVICE_ROOT_CONFLICT`.

An entry whose provider offer is `external_protocol` (DCP Claim §4) is an
`external` entry whatever its deployment kind, like an `external_saas` or
`webapp` binding: it is listed with reason `EXTERNAL_SURFACE`, never probed,
and never a documentation gap.

Every observation is applied. Documents change exactly when documented content
changes, for example served surfaces. The report changes whenever reasons,
details or observation times change.

Every entry that is not documented appears in the documentation report with a
reason code. Documentation MUST NOT be pruned silently.

An entry that should be documented but whose provider offer has no
`documentation` schemas is a documentation gap. Generation MUST fail with
`DOCUMENTATION_GAP`, naming every gap, rather than infer a schema.

## 4. Output Families

Documentation projection MAY generate:

- OpenAPI 3.1.0 for HTTP APIs, one document per provider component, holding its
  documented DCP capabilities and its shipped product API operations;
- human-readable capability summaries covering every entry;
- the documentation report.

AsyncAPI, MCP/tool documentation and SDK metadata are reserved output kinds. A
projector MUST NOT emit them until their owning offer kinds define
documentation fields.

## 4A. Embedding

Shipped schemas are embedded with their references rebased, so every `$ref` in
the output resolves:

- an offer schema's `#/<pointer>` becomes
  `#/components/schemas/<name>/<pointer>`, where `<name>` is
  `<capability>.<operation>.request` or `.response`;
- a product document's `#/components/<section>/<name>` becomes
  `#/components/<section>/<api_id>.<name>`.

Shipped schemas therefore use only JSON-Pointer references, with no `$id`,
anchor or dynamic-reference keywords. A reference that does not resolve after
rebasing fails rendering.

A capability with several operations is rendered at the envelope level:

- the request body is a `oneOf` of per-operation envelopes, each fixing
  `invocation.operation` with `const`;
- each operation's result is a `oneOf` of its success branch (`success: true`,
  with `output` from the operation's response schema) and a shared failure branch
  (`success: false`, `errorCode` required, `output` any object), so a fault never
  fails the success schema;
- with several operations, the response is an `anyOf` of the per-operation results;
- `x-dcp-operations` maps each operation to its pair.

A component that serves one capability under several contracts renders it as
one operation (one path, one offer): `x-dcp-contracts` lists the contract ids in
sorted order, and the envelope's `invocation.contractId` is an `enum` of them.
Hosted capabilities are grouped the same way, by provider component and
capability, with a sorted `contracts` list. Grouped entries that disagree on
the provider, description, operations or faults fail rendering with
`CAPABILITY_DOCUMENTATION_CONFLICT`. The report keeps one entry per contract.

Every document also describes `GET /dcp/registrations` (DCP HTTP Binding §4).
Its `200` response is the registration report, embedded as the schema
`dcp.registrations.report`, with a `Cache-Control: no-store` header.

Capabilities a host runs in process are rendered in the host's document under
`x-dcp-hosted-capabilities`, from the hosted components' own verified claims
and artifacts. Only currently registered capabilities are rendered. A host that
keeps admission evidence for a revoked contract is not in error.

## 5. Filtering

Documentation MUST be filterable by tenant, environment, contract, runtime
binding, and accepted capability surface. Filtering narrows a view; it never
changes an entry's status.

## 6. Security Schemes

Security is rendered per operation.

- **The verified set contains an identity contract.** A protected provider's
  DCP operations, and its product operations marked
  `x-dcp-access: protected`, carry an OAuth 2.0 bearer requirement. Its
  `GET /dcp/registrations` also names `x-dcp-permission: dcp.registrations.read`. Operations
  marked `x-dcp-access: public` carry an explicit empty requirement
  (`security: []`).
- **It does not.** No operation carries security, and the output states that the
  composition runs unauthenticated on a trusted network.

Configuration never decides this.

## 7. Sensitive Data

Generated documentation MUST NOT include raw credentials, secret references,
private prompts, retrieved chunks, provider secrets, or raw model outputs.
Endpoints appear only as the runtime binding declares them.

## 8. Aggregation

The projection walks the verified containment closure. Each child is documented
under its own provider component; an aggregate parent adds no surfaces of its
own.

## 9. Change Impact

This revision makes the projection executable: it adds typed, hash-pinned
payload schemas on offers and product API documents on claims (DCP Claim), the
DCP HTTP binding and registration report (DCP HTTP Binding), the documentation
decision, the report and its reason codes, and the per-component OpenAPI output.
It preserves the rule that docs expose only accepted DCP capability surfaces.
