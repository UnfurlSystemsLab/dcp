# Element: api_documents

**Owning spec:** [DCP Claim](../specs/DCP-CLAIM.md), projected by
[DCP Documentation](../specs/DCP-DOCUMENTATION.md)
**Plane:** Description and documentation projection.

## Meaning

`api_documents` ship OpenAPI descriptions of a component's product HTTP routes:
the routes it serves that are not DCP capability invocations, such as an
operations or management API.

## Required

No. A component without product routes omits it.

## Writers And Readers

Component authors write the OpenAPI document and the entry. Documentation
projectors read them. Resolvers and contract generators ignore them.

## Fields

- `api_id`: kebab-case, unique within the claim.
- `description`: human-readable purpose.
- `openapi`: an [artifact_ref](artifact-ref.md) to an OpenAPI 3.1.0 document.
- `surfaces`: non-empty kebab-case names of the served surfaces the document
  covers.

## Rules

- Every operation carries `x-dcp-surface` naming one of `surfaces`.
- Every operation carries `x-dcp-access`, either `public` (never needs a token)
  or `protected` (needs a bearer token whenever the composition's identity
  contract protects this component). There is no default. `x-dcp-permission`
  optionally names the permission the host checks.
- Every `$ref` targets the document's own `#/components/<section>/<name>`. When
  the projection merges the document it renames each component to
  `<api_id>.<name>` and rewrites the references to match.
- Two documents of one component never declare the same method and path.
- When two documents declare the same path, every path-level member they share other than `parameters` (for example
  `summary`) is identical; path-level `parameters` apply only to their own document's operations.
- No path starts with `/dcp/`; DCP invocation and registration paths are
  projected from offers and the DCP HTTP binding.
- The document declares no `servers`, `security` or
  `components.securitySchemes`. The projection derives them from the runtime
  binding and the composition's identity contract.
- Paths are relative to the component's service root.

## Default Behavior

None. Absent means the component documents no product routes.

## Composition Behavior

A document is projected only for a component that provides at least one
documented HTTP child contract, because only that contract's runtime binding
gives the component's service root. Only operations whose surface the running
host reports as served are included. The others appear in the documentation
report as `SURFACE_NOT_SERVED`. A child's documents describe only that child.

## Invalid Values

- An operation without `x-dcp-surface`, or naming an undeclared surface.
- An operation without `x-dcp-access`.
- A `$ref` to another file, a URL, or a non-`components` location.
- A `/dcp/...` path.
- A `servers` or security declaration.
- Using an API document to stand in for an offer another component must call.

## Examples

See [simple-component-claim.yaml](../examples/simple-component-claim.yaml).
