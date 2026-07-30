# DCP Specification Repository Rules For Codex

These rules apply to every Codex session working in this repository. They are
mandatory for any change to the Domain Claim Protocol specifications, schemas,
examples, or documentation.

## 1. Repository Role

This repository is the public specification home for DCP.

- Keep protocol specifications, schema narratives, examples, version notes, and
  discussion-oriented documents here.
- Keep Java implementation details in
  [`UnfurlSystemsLab/unfurl-dcp`](https://github.com/UnfurlSystemsLab/unfurl-dcp).
- Do not add Java source, Maven build plans, implementation-only package design,
  generated class docs, or runtime adapter code to this repository.
- If a spec change requires implementation work, document the implementation
  impact and point to `unfurl-dcp`; do not solve it locally with implementation
  content.

## 2. Specification First

DCP docs must introduce the specification before the schema.

When adding or changing protocol behavior, update documents in this order:

1. **Specification/index layer**: define the spec name, purpose, status, scope,
   and relationship to other DCP specs.
2. **Normative model layer**: describe the concept, lifecycle, invariants,
   required behavior, and composition rules in plain language.
3. **Schema layer**: define fields, types, required/optional status,
   cardinality, and validation rules.
4. **Element documentation layer**: document each field or element with purpose,
   allowed values, examples, edge cases, and cross-references.
5. **Examples layer**: add or update minimal and realistic examples.
6. **Version/change layer**: record breaking changes, migrations, and open
   questions.

A schema field must not appear without a prior normative concept. A normative
concept must not remain undocumented at the element/example level once it has a
schema surface.

## 3. Normative Versus Informative Text

Be explicit about what is binding.

- Use `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, and `MAY` only for normative
  requirements.
- Mark explanatory material, rationale, examples, diagrams, and implementation
  notes as informative when they are not binding.
- Do not hide requirements inside examples.
- Do not use vague requirement words such as "usually", "normally", or
  "basically" where a protocol rule is intended.

## 4. DCP Concepts Stay Product-Neutral

The spec must remain product-neutral even when examples mention Unfurl products.

- Model cross-cutting concerns as DCP claims, ports, runtime bindings, faults,
  policies, or composition rules before discussing product behavior.
- Do not hardcode Fabric, Flow, Foundry, Spring AI, Tomcat, GitHub, a cloud
  vendor, an identity provider, or a database into the core protocol.
- Product-specific behavior belongs in examples, non-normative notes, or the
  implementation repository.
- Keep child/referenced DCP aggregation as a first-class protocol construct.
  Do not introduce sidecar extension blocks that bypass the DCP reference model.

## 5. Schema Change Rules

Before changing any schema:

- Identify the owning specification section.
- Identify affected elements, examples, and compatibility/version notes.
- Decide whether the change is additive, tightening, renaming, removal, or a
  semantic change.
- Update examples so readers can see the new shape in context.
- Record open questions when the design is intentionally unresolved.

For fields:

- Prefer stable `snake_case` names.
- Prefer domain language over implementation language.
- Define required/optional status explicitly.
- Define default behavior explicitly, or state that there is no default.
- Define validation failure behavior where it affects composition or runtime
  binding.

## 6. Element Documentation Rules

Every public schema element must have documentation that answers:

- What does this element mean?
- Which plane owns it: description, negotiation, invocation, deployment, or
  documentation projection?
- Is it required?
- Who writes it?
- Who reads it?
- How does it compose with child/referenced DCPs?
- What are valid values?
- What are invalid values?
- What happens when it is missing or invalid?
- Which examples demonstrate it?

## 7. Examples Are Part Of The Spec

Examples are not decorative.

- Keep one minimal example for each major artifact.
- Keep at least one realistic aggregate example that uses child/referenced DCPs.
- Update examples in the same change as schema or normative behavior changes.
- Never use fake success examples that violate the current spec.
- Prefer small, readable examples over giant end-to-end dumps.

## 8. Versioning And Compatibility

DCP is draft, but changes must still be traceable.

- Record breaking changes in a version/change document once that structure
  exists.
- Until then, add a short "Change impact" note to the touched spec section.
- Do not preserve backward compatibility by default unless a published spec
  version explicitly requires it.
- Do not silently rename, remove, or repurpose a field. Explain the migration.

## 9. Discussion And Review

For major changes, include a review trail.

- Link to the GitHub Discussion or Issue when a change resolves a public design
  question.
- If no discussion exists yet, add an open question rather than pretending the
  design is settled.
- Prefer concrete adapter/component examples when arguing for a new construct.

## 10. Quality Bar Before Finishing

Before completing a spec change:

- The conceptual spec, schema, element docs, examples, and version notes are
  consistent.
- No Java implementation detail has leaked into the spec as a requirement.
- New terms are defined before use.
- Links point to canonical repositories:
  - DCP spec: `https://github.com/UnfurlSystemsLab/dcp`
  - Java implementation: `https://github.com/UnfurlSystemsLab/unfurl-dcp`
- Any validation, formatting, or link checks available in the repo have been
  run, or the reason they were not run is reported.

## 11. Current Transitional State

The current repository was seeded from earlier `unfurl-dcp` documents. Until the
new spec-first structure is created, treat these files as the source material to
be reorganized, not as the final information architecture:

- `docs/HLD-C-dcp-v0.2-internal.md`
- `docs/HLD-C2-dcp-schema-spec-updated.md`

When restructuring, preserve meaning first, then improve shape. Do not delete
technical content merely because it has not yet been moved into the final
spec/index/schema/element/example layout.
