# Host integration through the Arc Scene contract

This area covers how Arc products render through RenderBuilder: sessions, incremental updates,
frame streaming, crash isolation and the use of host semantics in looks. The design is in
[../../architecture/arc-scene-integration.md](../../architecture/arc-scene-integration.md).

Domain objects: session, host, snapshot, delta, revision, frame stream, render request.

## Sessions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOST-001 | Arc Scene sessions | Arc products start or connect to a RenderBuilder session process and send an Arc Scene snapshot; the renderer never runs inside the host process. | T1 | 4 |
| RB-HOST-002 | Contract negotiation | The handshake negotiates the contract version and capability set; a host and renderer with the same major version always interoperate, and missing optional capabilities are reported to the host. | T1 | 4 |
| RB-HOST-003 | Incremental updates | Hosts send deltas keyed by stable identifiers (add, modify, remove prims, materials, lights, cameras and settings) tagged with revision numbers; deltas apply in revision order and frames rendered from an older revision are discarded. | T1 | 4 |
| RB-HOST-004 | Frame streaming | Progressive frames stream back over shared memory and are shown in the host's own viewport or panel; the host chooses outputs and update rate. | T1 | 4 |
| RB-HOST-005 | Crash isolation | A renderer fault never affects the host. The host shows the error, and a restarted session rebuilds from the last snapshot and acknowledged deltas. | T1 | 4 |
| RB-HOST-006 | Local-only security | Sessions listen only on local IPC endpoints protected by a per-session token; remote rendering goes through Arc Hub, never through an open network port. | T1 | 4 |
| RB-HOST-007 | Host resource control | The host sets thread count and priority for its session so the host stays responsive while rendering. | T1 | 4 |

## Rendering from hosts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOST-008 | Path-traced viewport mode | A host viewport switches to progressive path-traced display with the viewport camera and returns to real-time display at any time, with the same materials and lights. | T1 | 4 |
| RB-HOST-009 | Final renders from hosts | A host requests a render with a named setup; outputs return as files and as an image asset the host can place in its document, for example on a sheet. | T1 | 4 |
| RB-HOST-010 | Host-stored settings | Hosts store render setups and look choices in their own documents as Arc Scene render settings, so a model renders the same way without a separate render document. | T1 | 4 |
| RB-HOST-011 | Live link documents | A `.renderb` document can reference a running host session or an exported Arc Scene package as its scene source and layer looks and overrides over it. | T1 | 4 |
| RB-HOST-012 | Saved view binding | Render setups bind to host saved views, so changing the host view updates the setup camera. | T2 | 4 |
| RB-HOST-013 | Headless host renders | Host CLIs request renders through a session without any window, for example rendering saved views in continuous integration. | T2 | 4 |

## Semantics and selection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOST-014 | Host materials | OpenPBR-aligned materials from the host render directly; looks can override them per element, category or any collection. | T1 | 4 |
| RB-HOST-015 | Semantic attributes | Host semantics (IFC class, category, level, system, layer, assembly, phase) arrive as prim attributes usable in selector expressions, for example assigning glazing to every window. | T1 | 4 |
| RB-HOST-016 | Selection sync | Selecting in the render view selects the element in the host and the reverse. | T2 | 4 |
| RB-HOST-017 | Multi-host sessions | Several hosts contribute scenes to one session, for example a building and its site from two products, aligned through their coordinate reference records. | T3 | 5 |
