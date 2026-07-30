# Element: metadata

**Owning specs:** All DCP artifact specs.
**Plane:** Description, negotiation, runtime binding, and documentation
projection.

## Meaning

`metadata` records protocol version, artifact version, provenance, lifecycle, and
references needed to review or verify a DCP artifact.

## Required

Yes for claims, contracts, and runtime bindings. Required fields depend on the
artifact type.

## Writers And Readers

Artifact authors, compilers, signers, and runtime binders write metadata.
Catalogs, verifiers, operators, and documentation projectors read it.

## Common Fields

- `dcp_version`
- artifact version such as `claim_version` or `binding_version`
- `supersedes`
- `effective_from`
- `references`
- provenance and trust records

## Composition Behavior

Metadata supports verification, drift detection, compatibility checks, and
documentation projection. Parent DCPs should preserve child artifact metadata
when projecting aggregate views.

## Invalid Values

- Missing DCP version.
- Untraceable generated artifact.
- Metadata that claims trust or signatures without corresponding evidence.

## Examples

See all files in `docs/examples/`.
