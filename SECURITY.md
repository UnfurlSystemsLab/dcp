# Security Policy

DCP is a draft protocol for describing component claims, composition contracts,
runtime bindings, documentation projections, and related governance surfaces.
Security reports should focus on protocol or repository behavior that could lead
to unsafe composition, unsafe disclosure, or unsafe runtime binding.

## Supported Versions

| Version | Supported |
|---|---|
| `0.2.0-draft` | Yes |

Draft versions may change quickly, but security-relevant changes must still be
recorded in the changelog and reflected in affected specs, schemas, examples,
and compatibility notes.

## What To Report

Please report issues such as:

- protocol shapes that encourage leaking secrets, tokens, credentials, prompts,
  raw model outputs, retrieved chunks, or private customer data;
- runtime binding rules that allow raw secrets where secret/config references
  should be required;
- documentation projection behavior that exposes capabilities not accepted by a
  frozen DCP contract;
- auth, authorization, audit, telemetry, identity, trust, or policy gaps in DCP
  claims, ports, faults, or runtime bindings;
- aggregation behavior where referenced child DCPs can bypass parent governance;
- fault semantics that hide unsafe invalidation, downgrade, or retry behavior;
- examples that normalize unsafe production patterns.

## What Not To Include Publicly

Do not post secrets, credentials, private keys, access tokens, customer data,
private prompts, raw model outputs, retrieved private chunks, exploit payloads,
or proprietary deployment details in public Issues or Discussions.

If a report needs sensitive detail, use GitHub private vulnerability reporting
or a private maintainer channel when available. If neither is available, open a
public Issue with only a non-sensitive summary and ask maintainers to arrange a
private channel.

## Response Expectations

Maintainers should acknowledge security reports, classify whether the issue is a
protocol concern, implementation concern, or deployment concern, and identify
the repository that owns the fix.

Protocol fixes in this repository should update:

1. the relevant spec page;
2. the related schema and element documentation;
3. examples that demonstrate the safe shape;
4. changelog and compatibility notes;
5. implementation-impact notes for
   [`unfurl-dcp`](https://github.com/UnfurlSystemsLab/unfurl-dcp) when needed.

Implementation-only issues should be moved to the owning implementation or
product repository without redefining DCP behavior locally.
