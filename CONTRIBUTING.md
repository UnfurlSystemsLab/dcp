# Contributing To DCP

Thank you for helping improve the Domain Claim Protocol (DCP). This repository
is the public specification home for DCP, so contributions should improve the
protocol, its schemas, examples, compatibility notes, or public discussion
record.

The Java implementation belongs in
[`UnfurlSystemsLab/unfurl-dcp`](https://github.com/UnfurlSystemsLab/unfurl-dcp).
Implementation pull requests are welcome there, but protocol behavior should be
specified here first.

Unless you explicitly state otherwise, contributions submitted to this
repository are licensed under the [Apache License 2.0](LICENSE), matching this
repository's license.

## Before Proposing A Change

Read the relevant specification documents before opening a pull request:

1. [DCP Standards Support](docs/DCP-STANDARDS-SUPPORT.md)
2. [DCP Core](docs/specs/DCP-CORE.md)
3. The owning spec for the artifact you want to change
4. The related schema and element pages
5. [Compatibility](docs/versions/compatibility.md)

If the change is still exploratory, start with a
[GitHub Discussion](https://github.com/UnfurlSystemsLab/dcp/discussions). Use an
Issue when you already have a concrete defect, missing field, broken example, or
documentation gap.

## How To Propose Spec Changes

Good spec proposals should explain:

- the real adapter, component, or runtime scenario that exposed the gap;
- the DCP plane affected: description, negotiation, invocation, deployment, or
  documentation projection;
- the existing spec section or schema field that is insufficient;
- the proposed normative behavior;
- how the change composes with child/referenced DCPs;
- whether the change is additive, breaking, or a clarification;
- what implementation impact is expected for `unfurl-dcp` or product repos.

Pull requests should keep product-specific examples clearly marked as examples.
Do not define protocol behavior only through Fabric, Flow, Foundry, Spring AI,
GitHub, a cloud provider, or another implementation.

## How To Add Or Change Schemas

DCP is spec-first. Schema changes must follow this order:

1. Update the owning specification page under `docs/specs/`.
2. Update or add the schema under `docs/schemas/`.
3. Update the affected element page under `docs/elements/`.
4. Update examples under `docs/examples/`.
5. Update [the changelog](docs/versions/changelog.md).
6. Update [compatibility notes](docs/versions/compatibility.md) when the change
   breaks, renames, removes, or repurposes a field.

Every schema field must define:

- purpose and owning plane;
- required or optional status;
- writer and reader;
- valid and invalid values;
- default behavior, or a statement that there is no default;
- validation failure behavior;
- composition behavior with child/referenced DCPs;
- examples that show the field in context.

## Examples And Changelog Are Required

Examples are part of the specification. If a pull request changes protocol
shape, negotiation behavior, runtime binding, documentation projection, or fault
semantics, it must update examples in the same change.

The changelog is also required. Record:

- added, changed, removed, or deprecated behavior;
- breaking impact and migration notes;
- unresolved design questions that should move to Discussions.

## Pull Request Checklist

Before submitting:

- The relevant spec, schema, element docs, examples, and version notes agree.
- Normative requirements use `MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, or
  `MAY` intentionally.
- Informative rationale and examples are not mistaken for binding rules.
- No product-specific behavior is hidden inside the core protocol.
- The change links to a Discussion or Issue when it resolves a public design
  question.
- Sensitive data, credentials, proprietary prompts, raw model outputs, and
  retrieved private chunks are not included.

## Where To Discuss

- [DCP Discussions](https://github.com/UnfurlSystemsLab/dcp/discussions): design
  questions, tradeoffs, proposals, and adapter experience.
- [DCP Issues](https://github.com/UnfurlSystemsLab/dcp/issues): concrete
  defects, broken examples, missing fields, or documentation fixes.
- [unfurl-dcp](https://github.com/UnfurlSystemsLab/unfurl-dcp): Java
  implementation work that follows accepted protocol behavior.
