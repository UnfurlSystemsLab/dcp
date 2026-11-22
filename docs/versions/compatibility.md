# DCP Compatibility

**Status:** Draft.

DCP is not externally released as a stable standard yet. Compatibility rules are
therefore designed for traceability and clear migration, not long-term backward
compatibility guarantees.

## Current Compatibility Policy

- Draft changes MAY break earlier draft shapes.
- Breaking changes MUST be recorded in `changelog.md`.
- Renamed, removed, or repurposed fields MUST include migration guidance.
- Examples MUST be updated in the same change as schema or normative behavior
  changes.
- The Java implementation repository MAY provide temporary migration adapters,
  but the spec repo does not preserve legacy fields by default.

## Version Labels

- `0.2.0-draft`: current draft family seeded from the original HLD-C and HLD-C2
  documents.
- Future public releases SHOULD use stable semantic version labels and describe
  compatibility expectations explicitly.

## Artifact Compatibility

| Artifact | Compatibility rule |
|---|---|
| Claim | `metadata.dcp_version` and `metadata.claim_version` identify the protocol and claim version. |
| Contract | `contract_version` identifies the frozen contract shape. |
| Runtime binding | `binding_version` identifies the binding artifact shape. |
| Documentation projection | Generated docs must identify the contract/binding they were generated from. |

## Implementation Compatibility

`unfurl-dcp` is the Java implementation of this specification. If the Java
implementation accepts legacy aliases or migration helpers, that behavior should
be documented in `unfurl-dcp`, not in the protocol spec as a permanent rule.

## Documentation Projection Migration (Unreleased)

| Earlier field | Replacement | Migration |
|---|---|---|
| `source_contract` | `source.root_contract` | Use the verified aggregate root id. |
| `source_binding` | `source.runtime_bindings` | List every child binding id used. |
| `outputs[].kind/path` | `surfaces[]` plus the rendered views | Output locations are a projector concern, not part of the model. |
| `filters` | Request-time filtering (DCP Documentation §5) | Filters narrow a view and are not stored. |
| `schemas[].request_schema_ref` / `response_schema_ref` (HLD-C2 G draft) | `components[].capabilities[].request_schema` / `response_schema` (`artifact_ref`) | Ship the schemas in the component artifact and reference them from `offers[].documentation`. |

Claims that carried payload schemas as `interface.details.input_schema` /
`output_schema` path strings move them to `offers[].documentation`, with
`sha256` added. The detail keys are not documentation proof.
