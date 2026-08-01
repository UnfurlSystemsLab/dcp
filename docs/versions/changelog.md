# DCP Changelog

This file records public specification changes for the DCP repository.

## 0.2.0-draft

### Changed

- Expanded the repository README with a reader-facing "What is DCP?" section,
  including why DCP exists, the three planes, core artifacts, and what DCP is
  not.
- Added README navigation tabs and a quick-path map so readers can move from
  overview to specs, schemas, elements, examples, versions, and discussions.

### Added

- Added the root MIT `LICENSE` file so GitHub can expose the repository's
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
