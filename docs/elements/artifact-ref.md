# Element: artifact_ref

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md)
**Plane:** Description and documentation projection.

## Meaning

An `artifact_ref` names one file shipped inside the component's own artifact
(the artifact that carries its packaged claim), pinned by content hash. It is a
value type used by [offers](offers.md) `documentation` and
[api_documents](api-documents.md).

## Required

Required wherever a field's type is `artifact_ref`. All three fields are
required.

## Writers And Readers

The component's build writes the ref and the file together, computing the hash
from the file it packages. Composers copy the file into the deployment's
content-addressed store. Documentation projectors read and verify it.

## Fields

- `path`: relative path inside the component artifact, with `/` separators,
  preferably under `dcp/`.
- `media_type`: `application/schema+json`, `application/vnd.oai.openapi+json`,
  or `application/vnd.oai.openapi+yaml`.
- `sha256`: lower-case hex SHA-256 of the file's exact bytes.

## Default Behavior

There is no default. A path is never resolved against any other base, and a ref
without a hash is invalid.

## Validation Failure Behavior

A missing file, a hash mismatch, an unsafe path, or a media type the using field
does not allow is a validation failure. Readers never fall back to another copy,
another version, or an inferred document.

## Composition Behavior

Each claim's refs resolve only inside that claim's own artifact. In a deployment
bundle the file is stored as `artifacts/sha256/<sha256>`, and readers resolve it
by hash; the claim is never rewritten. A parent never resolves a child's ref, or
the reverse.

## Invalid Values

- `path: ../schemas/input.json` (leaves the artifact).
- `path: /dcp/schemas/input.json` (absolute).
- `path: https://example.com/schema.json` (remote).
- `sha256` with upper-case letters or fewer than 64 characters.
- JSON Schemas with a remote `$ref`, or using `$id`, `$anchor`, `$dynamicRef`,
  `$dynamicAnchor` or `$recursiveRef`. Only JSON-Pointer fragments (`#/...`)
  are allowed, so the projection can rebase them when it embeds the schema.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
