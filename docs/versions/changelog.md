# DCP Changelog

This file records public specification changes for the DCP repository.

## Unreleased (0.2.0-draft family)

### Added

- `offers[].documentation` on claims: optional `summary`, plus `request_schema` and
  `response_schema` as hash-pinned JSON Schemas of the invocation payloads
  (HLD-C2 A.6; DCP Claim §4).
- The `artifact_ref` value type: a file inside the component's own artifact,
  named by relative `path`, `media_type` and `sha256` (HLD-C2 A.12; element
  `artifact_ref`).
- Optional claim section `api_documents`: hash-pinned OpenAPI 3.1.0 documents
  for a component's product HTTP routes, with operations tagged by
  `x-dcp-surface` (HLD-C2 A.13; element `api_documents`).
- New spec page DCP HTTP Binding, covering service roots,
  `POST <service_root>/dcp/<capability>`, and
  `GET <service_root>/dcp/registrations`, with both identity paths (HLD-C2 G.3).
- New schemas `registration-report.schema.yaml` and
  `http-invocation.schema.yaml`.
- Documentation projection: the projection inputs, the documentation decision,
  documentation gaps, the per-component OpenAPI output, the documentation report
  and its reason codes (HLD-C2 G.1, G.2, G.4).
- Runtime binding: the endpoint of an HTTP-served provider is
  `<service_root>/dcp/<provider_capability>` (HLD-C2 C; DCP Runtime Binding §3A).
- Examples: `registration-report.yaml` and `documentation-projection.yaml`. The
  simple claim example now shows `documentation` and `api_documents`.
- Registration reports carry per-registration provider evidence
  (`provider_claim_version`, `provider_claim_hash`). The projection checks the
  host and each provider separately (`HOST_CLAIM_MISMATCH` vs `CLAIM_MISMATCH`),
  so components hosted in process are verified against their own pins.
  `SERVICE_ROOT_CONFLICT` rejects two pinned providers on one root.
- Per-operation security: `api_documents` operations declare
  `x-dcp-access: public | protected` and optionally `x-dcp-permission`. Public
  operations render `security: []` when identity is in the composition.
- Reference rebasing for embedded schemas. Shipped schemas use only JSON-Pointer
  `$ref`s, with no `$id`, anchor or dynamic references.
- Every observation is applied; documents change exactly when documented content
  changes.
- Offer documentation is per operation:
  `documentation.operations[{operation, request_schema, response_schema}]`, with
  object-rooted schemas. The projection selects the operation at the envelope
  level (`oneOf` on `invocation.operation`, `x-dcp-operations`).
- Hosted in-process capabilities are rendered in the host's document under
  `x-dcp-hosted-capabilities`. The host view takes the hosted components'
  verified claims and artifacts as inputs (`HOST_VIEW_INPUT_MISMATCH`). Only
  currently registered capabilities are rendered; admission evidence kept for a
  revoked contract is not an error.
- Result envelopes have separate success and failure branches; the operation's
  response schema applies only when `success` is true.
- Shared path metadata across a component's API documents must be identical
  (`API_DOCUMENT_PATH_CONFLICT`). The projection output model records each
  component's `name` and `protected` flag and each capability's `description`
  and `provider_component_version`; `root_contract` and `binding_id` are absent
  in a host view.

- Every rendered OpenAPI document describes `GET /dcp/registrations`, with the
  registration report schema `dcp.registrations.report` and, for a protected
  component, the bearer requirement and `x-dcp-permission:
  dcp.registrations.read` (HLD-C2 G.2; DCP Documentation §4A).
- `registrations` is a reserved capability name (HLD-C2 G.3; DCP HTTP Binding §4).

### Changed

- HLD-C2 G.2 states that enumeration values in the projection and the
  registration report are written in lower case, as the schemas list them, and
  reason codes in upper case.
- `documentation-projection.schema.yaml` now models the HLD-C2 G.2 output
  (`source.root_contract`, `identity`, `components`, `report`) in place of the
  earlier `source_contract` / `source_binding` / `outputs` / `filters` sketch.
  See compatibility notes.
- DCP Documentation: free-form `interface.details` schema hints,
  `metadata.references`, environment variables and network or orchestrator
  discovery are listed as non-proof.

### Change Impact

Claim changes are additive. Absent fields are not serialized, so existing claims
keep their canonical hashes and existing contract pins stay valid. Hosts that
serve DCP over HTTP gain one required route, `/dcp/registrations`. The
documentation projection schema change is breaking for anything that produced
the earlier sketch.

## 0.2.0-draft

### Changed

- Changed the DCP repository license from MIT to Apache License 2.0 before
  external release so the specification and Java implementation share one
  permissive license with explicit patent terms.
- Expanded the repository README with a reader-facing "What is DCP?" section,
  including why DCP exists, the three planes, core artifacts, and what DCP is
  not.
- Added README navigation tabs and a quick-path map so readers can move from
  overview to specs, schemas, elements, examples, versions, and discussions.

### Added

- Added `CONTRIBUTING.md` with the DCP spec-change, schema-change, example, and
  changelog requirements.
- Added `SECURITY.md` with DCP-specific security reporting scope for claims,
  runtime bindings, documentation projection, faults, aggregation, and
  governance concerns.
- Added the root Apache 2.0 `LICENSE` file so GitHub can expose the repository's
  license overview tab.
- Created the spec-first public document structure:
  - standards-support index;
  - per-topic specification pages;
  - schema directory;
  - element reference directory;
  - examples directory;
  - version and compatibility notes.
- Added DCP aggregation as an explicit specification page for child/referenced
  DCP composition.
- Added DCP documentation projection as an explicit specification page for
  exposing only accepted capability surfaces.
- Added `docs/DCP-VOCABULARY.md` as an informative vocabulary page that maps
  common DCP terms back to their owning specs and element pages.
- Added minimal claim, aggregate claim, and runtime binding examples.

### Preserved

- Existing seeded source documents remain available:
  - `docs/HLD-C-dcp-v0.2-internal.md`
  - `docs/HLD-C2-dcp-schema-spec-updated.md`

### Change Impact

This is an information-architecture change. It does not intentionally change
DCP field names or protocol semantics from the current v0.2 draft.

## Open Questions

- Which element pages should be split next into narrower field-level pages?
- Should the schema files use a formal JSON Schema dialect or remain readable
  YAML schema notes until the field set stabilizes?
- Which GitHub Discussions should be linked from each major spec page once
  public review begins?
