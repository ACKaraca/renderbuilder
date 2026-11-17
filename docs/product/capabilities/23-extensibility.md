# Extensibility

This area covers how users and developers extend RenderBuilder: automation through commands,
custom shading through MaterialX and OSL, and sandboxed plug-ins for render stages that run on the
CPU side of the renderer.

Domain objects: command, protocol event, node definition, plug-in, plug-in manifest.

## Automation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-EXT-001 | Render commands | Every render operation (open, edit setups, assign looks, render, cancel, snapshot, package) is an Arc command available from the UI, the command line, macros and the automation protocol. | T1 | 4 |
| RB-EXT-002 | Python client | The official Python client renders, edits setups and reads statistics out of process through the automation protocol. | T1 | 4 |
| RB-EXT-003 | Event subscriptions | Protocol clients subscribe to progress, pass completion, frame completion, warnings and errors. | T1 | 4 |
| RB-EXT-004 | Render job graphs | Node graphs on the suite node engine generate setups and job variants, for example a study per facade option. | T3 | 5 |

## Custom shading

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-EXT-005 | Custom nodes from MaterialX | New nodes are defined as MaterialX node definitions with node graph implementations and run on every device. | T1 | 4 |
| RB-EXT-006 | OSL shaders | OSL shaders compiled to `.oso` translate into the shading program within a documented subset; operations outside it are reported. | T2 | 5 |

## Plug-ins

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-EXT-007 | Sandboxed plug-ins | WebAssembly plug-ins with a manifest, no ambient authority and memory and time limits extend procedural generation, imagers, output writers and scene readers. A failing plug-in fails its task without affecting the process. | T2 | 5 |
| RB-EXT-008 | Procedural plug-ins | Procedural plug-ins emit prims through a bounded API (RB-GEOM-023). | T2 | 5 |
| RB-EXT-009 | Imager plug-ins | Imager plug-ins receive output channels and return images (RB-IMGR-014). | T3 | 5 |
| RB-EXT-010 | Output writer plug-ins | Writer plug-ins receive finished frames and channels and write additional file formats or streams. | T3 | 5 |
| RB-EXT-011 | Scene reader plug-ins | Reader plug-ins translate additional input formats into prims, with loss reports. | T3 | 5 |
| RB-EXT-012 | Plug-in provenance | Plug-in names, versions and digests are recorded in output metadata and render packages so renders stay reproducible. | T2 | 5 |
