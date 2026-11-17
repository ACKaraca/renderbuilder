# Host applications

This area covers running RenderBuilder inside other content-creation applications: a Hydra render
delegate for any Hydra-capable host, and host integrations (connectors) that translate a host's
scene to the Arc Scene contract through the host's public API. Sessions, deltas and frame streaming
are specified in [19-host.md](19-host.md); the design is in
[../../architecture/arc-scene-integration.md](../../architecture/arc-scene-integration.md#host-applications).
The decision to use connectors is suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md);
the product decision is [ADR 0007](../../adr/0007-delegate-and-connectors-without-the-engine.md).

Domain objects: host application, render delegate, connector, translation, loss report, session,
render view.

## Hydra render delegate

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOSTAPP-001 | Hydra render delegate | A render delegate plug-in registers RenderBuilder as a renderer in any Hydra-capable host, selectable in the host's renderer list like any other delegate, and renders the host's viewport with the host's camera. | T3 | 5 |
| RB-HOSTAPP-002 | Delegate without the engine | The delegate contains no engine code. It starts or connects to a session process over a local endpoint with a session token, so a renderer fault never terminates the host (RB-HOST-005, RB-HOST-006), and it runs against the session of the matching contract version (RB-HOST-002). | T3 | 5 |
| RB-HOSTAPP-003 | Hydra scene coverage | Meshes, subdivision meshes, curves, points, volumes, instancers, materials, lights, cameras and render settings arrive through Hydra change tracking as an Arc Scene snapshot and deltas keyed by prim path. Materials expressed as MaterialX or UsdPreviewSurface networks render from their own definition; anything else is replaced by the diagnostic material with a loss entry. | T3 | 5 |
| RB-HOSTAPP-004 | Hydra render buffers | Hydra render buffers and AOV bindings map to RenderBuilder outputs; id buffers support picking in the host; the delegate reports convergence and honors the host's stop, pause and restart calls. | T3 | 5 |
| RB-HOSTAPP-005 | Delegate settings | Sampling, depth limits, device, denoising and integrator mode appear as the delegate's render settings and map to a render setup; unknown settings are ignored and listed in the loss report. | T3 | 5 |

## Host connectors

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOSTAPP-006 | Host connector | A connector runs inside the user's own licensed copy of another application, uses only that application's public API, and drives a RenderBuilder session through the Arc Scene contract. It contains no engine code and is written in the language the host requires. | T3 | 5 |
| RB-HOSTAPP-007 | Scene translation | The connector translates the host's geometry, transforms, hierarchy, instancing and visibility into an Arc Scene snapshot, with stable identifiers derived from the host's persistent element identifiers so that looks and overrides survive reloads. | T3 | 5 |
| RB-HOSTAPP-008 | Material translation | Host materials translate to the Arc Scene material model (OpenPBR parameters or MaterialX networks) where the host API exposes their definition, including texture references, scale and projection. Materials that cannot be expressed are replaced by the diagnostic material with a loss entry; they are never approximated silently. | T3 | 5 |
| RB-HOSTAPP-009 | Lights and cameras | Host lights, photometric profiles, sun and sky systems and cameras translate with their physical parameters (units, exposure, lens, shutter, depth of field), so the same lit shot renders from the host and from a render package. | T3 | 5 |
| RB-HOSTAPP-010 | Render settings and outputs | The host's render settings, render passes and output channels map to a render setup and its outputs; the connector presents RenderBuilder's settings in the host's own settings interface where the host allows it. | T3 | 5 |
| RB-HOSTAPP-011 | Interactive updates | Host change notifications become deltas keyed by stable identifiers and tagged with revisions, coalesced per pass, so editing in the host updates the running render without re-export (RB-HOST-003). | T3 | 5 |
| RB-HOSTAPP-012 | Render view inside the host | Progressive frames display in a panel, window or frame buffer of the host, with region render, pause, resume, snapshot and output switching, and the camera of the host's viewport or a chosen camera. | T3 | 5 |
| RB-HOSTAPP-013 | Host stand-ins and procedurals | A host proxy object references a stand-in or a procedural (RB-GEOM-022, RB-GEOM-023) and shows a bounding box, point sample or proxy mesh in the host viewport (RB-GEOM-024); the full content loads only in the render. | T3 | 5 |
| RB-HOSTAPP-014 | Batch export from the host | The host exports a render package or an Arc Scene package from the open scene, and renders in batch or submits to the self-hosted farm without a window, for example from a host's command-line mode (RB-BATCH-006, RB-BATCH-015, RB-HOST-013). | T3 | 5 |

## Compatibility and safety

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-HOSTAPP-015 | Connector negotiation | A connector reports the host application and API version and the features it can translate; a session reports its feature matrix; features missing on either side are listed to the user, not fatal (RB-HOST-002). | T3 | 5 |
| RB-HOSTAPP-016 | Translation loss reports | Every translation produces a loss report with one entry per host element that could not be represented: host identifier, what was lost, what replaced it. Reports are shown in the host, written as JSON and included in render packages (RB-IO-015). | T3 | 5 |
| RB-HOSTAPP-017 | Connector isolation | Connectors open no network port, read only the scene data a request needs, store nothing outside the host's document and the session's cache, and are never required by a core RenderBuilder workflow. | T3 | 5 |
