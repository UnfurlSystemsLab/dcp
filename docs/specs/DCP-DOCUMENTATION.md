# DCP Documentation Projection Specification

**Status:** Draft.
**Plane:** Documentation projection.
**Source material:** `docs/HLD-C2-dcp-schema-spec-updated.md`, section G.

## 1. Purpose

DCP documentation projection generates runtime-facing documentation from
accepted DCP surfaces only. It prevents documentation from exposing capabilities
that were not accepted by the claim/contract/binding chain.

## 2. Projection Rule

Generated documentation MUST be derived from accepted claims, offers, contracts,
and runtime bindings. It MUST NOT expose hidden implementation endpoints or
unaccepted provider capabilities.

## 3. Output Families

Documentation projection MAY generate:

- OpenAPI/Swagger for HTTP APIs;
- AsyncAPI for event streams;
- MCP/tool documentation for accepted tool surfaces;
- human-readable capability summaries.

## 4. Filtering

Documentation MUST be filterable by tenant, environment, contract, runtime
binding, and accepted capability surface.

## 5. Sensitive Data

Generated documentation MUST NOT include raw credentials, private prompts,
retrieved chunks, provider secrets, or raw model outputs by default.

## 6. Change Impact

This document gives generated documentation its own spec surface and preserves
the current rule that docs expose only accepted DCP capability surfaces.
