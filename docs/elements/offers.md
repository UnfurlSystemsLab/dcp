# Element: offers

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description, negotiation, and documentation projection.

## Meaning

`offers` define capabilities this component exposes for other components or
aggregates to consume.

## Required

Yes. The section MUST exist, but the list MAY be empty.

## Writers And Readers

Component authors write offers. Negotiators, contract generators, runtime
binders, documentation projectors, and consumers read them.

## Fields

- `capability`: stable capability name.
- `description`: human-readable explanation.
- `consumer_access`: `any` or `named_components_only`.
- `interface.kind`: `http_api`, `event_stream`, `negotiation`, `in_process`, or
  `external_protocol`. An `external_protocol` offer is reached over a standard
  protocol of the provider's own (for example OAuth 2.0 for `identity-context`),
  never through the DCP invocation envelope; its `details` name it in
  `protocol` (for example `oauth2`).
- `interface.details`: deterministic kind-specific map.
- `stability`: `experimental`, `evolving`, `stable`, or `deprecated`.
- `version`: semantic version.
- `cost_implications`: required when the capability uses metered resources.
- `documentation`: optional; required when the offer is documented. It holds
  `summary` (optional) and a non-empty `operations` list with one entry per
  `invocation.operation` value. Each entry has `operation`, an optional
  `summary`, and `request_schema` and `response_schema`, each an
  [artifact_ref](artifact-ref.md) to a JSON Schema (draft 2020-12) whose root is
  `type: object`, describing that operation's `input` and successful `output`.

## Composition Behavior

Plane 2 binds consumer dependencies to provider offers. Generated documentation
MUST expose only accepted offers.

`documentation` is never matched by the resolver and never changes which needs
an offer satisfies. It is covered by the claim hash, so a contract that pins the
claim also pins the schema bytes. Each child's offers are documented under that
child; a parent never re-documents a child's offer.

## Default Behavior

There is no default `documentation`. An offer without it has no documentable
payload. If the documentation projection is asked to document it, that is a
documentation gap and generation fails; a schema is never inferred.

## Invalid Values

- Missing version.
- Interface details that are only prose when deterministic fields are needed.
- Offering a capability that contradicts a refusal.
- Exposing hidden implementation endpoints that are not part of the DCP surface.
- Payload schemas given only as `interface.details` hints (for example an
  `input_schema` path string) instead of `documentation`.
- A `documentation` schema ref whose `media_type` is not
  `application/schema+json`, or whose `sha256` does not match the file.
- One schema for several operations that tries to branch on the operation name;
  the operation is outside `input`, so list one entry per operation instead.
- An `external_protocol` offer without a non-blank `details.protocol`, or with
  `documentation` (it has no DCP payload).
- A payload schema whose root is not an object (for example plain text); wrap
  it, as in `{"markdown": "..."}`.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml) and
[aggregate-dcp.yaml](../examples/aggregate-dcp.yaml).
