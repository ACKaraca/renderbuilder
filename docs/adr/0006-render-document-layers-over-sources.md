# 0006. The render document layers over scene sources

- Status: Accepted
- Capabilities: RB-SCENE-001, RB-SCENE-002, RB-SCENE-004, RB-SCENE-005, RB-OVR-001, RB-OVR-007

## Context

Scenes come from Arc host products, OpenUSD stages and render packages. Copying a scene into the
renderer's file would fork the model: edits in the host would not reach renders, and render-side
changes would be lost on re-export. Suite principle 3 states that the document is the authority and
render scenes are derived.

## Decision

- A `.renderb` document is an Arc document package (`arc-doc`) that stores references to scene
  sources plus everything RenderBuilder authors: setups, looks, override layers, document lights and
  cameras, materials, studies, snapshots and library links.
- Looks and overrides address prims by stable identity and collections; they are evaluated at
  render time and never written into sources.
- For transport, a render package captures a flattened OpenUSD copy of the resolved scene with
  MaterialX and content-addressed assets; it is a snapshot for reproducibility, not an editing
  format.
- Hosts may instead store render setups in their own documents as Arc Scene render settings
  (RB-HOST-010); a `.renderb` document is needed only for render-specific authoring.

## Consequences

- Host edits flow into renders without re-export; render edits survive source updates.
- Assignments can become orphaned when a source changes identity; they are reported, not deleted.
- Two places can hold render settings (host document and render document); the host decides which
  applies to its requests.

## Alternatives considered

- **Render document as an OpenUSD layer.** Natural composition and exchange, but undo, journal,
  recovery and Hub versioning come from the Arc document package; revisited under OQ-18.
- **Import-and-copy scenes.** Simplest storage, but forks the model.

## Reopen when

Most look and override data maps one to one onto OpenUSD constructs and users ask to edit render
documents in other OpenUSD tools.
