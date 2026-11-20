# Capability specification

The capability files define what RenderBuilder does, one row per testable capability. They are the
reference for issues, pull requests, workflows and the roadmap.

## Legend

| Item | Meaning |
|---|---|
| Identifier | `RB-<AREA>-<NNN>`, for example `RB-PATH-006`. `AREA` is the token of the file; numbers start at 001 in each file. Identifiers are permanent: a withdrawn capability keeps its number and its row is marked withdrawn. |
| Behavior | Observable behavior precise enough to implement and test: inputs, result, what updates automatically, limits. |
| Tier T1 | Core: required for first production use. |
| Tier T2 | Professional: required for parity with established professional renderers. |
| Tier T3 | Advanced: specialized or differentiating. |
| Phase | The suite phase in which the capability is planned (see [../../roadmap.md](../../roadmap.md)). RenderBuilder starts in phase 4; GPU rendering and production depth are phase 5. |

Implementation status is never recorded here. It lives in GitHub issues and the project board.

## Areas

| File | Token | Scope | Rows |
|---|---|---|---|
| [01-scene.md](01-scene.md) | SCENE | Render document, scene sources, scene graph, render attributes, collections, assets, render setups | 25 |
| [02-camera.md](02-camera.md) | CAM | Projections, optics, exposure and shutter, camera utilities | 22 |
| [03-geometry.md](03-geometry.md) | GEOM | Meshes, subdivision, displacement, NURBS and limit surfaces, curves, points, splats, instancing, stand-ins, procedurals | 31 |
| [04-volume.md](04-volume.md) | VOL | Grid volumes, media, atmosphere, volume shading and outputs | 13 |
| [05-light.md](05-light.md) | LIGHT | Light types, units, linking and groups, light filters, rigs | 28 |
| [06-surface.md](06-surface.md) | SURF | OpenPBR surface, interchange surfaces, layering, compositing and special materials | 21 |
| [07-hair-stylized.md](07-hair-stylized.md) | STYL | Hair and fur shading, toon, contours and technical illustration | 11 |
| [08-shading-nodes.md](08-shading-nodes.md) | NODE | Shading network model, compilation and utility nodes | 22 |
| [09-texture.md](09-texture.md) | TEX | Image textures, texture cache, procedural textures, baking | 17 |
| [10-transport.md](10-transport.md) | PATH | Integrator, bidirectional mode, light sampling, path guiding, determinism, devices, spectral mode | 27 |
| [11-adaptive-denoise.md](11-adaptive-denoise.md) | NOISE | Progressive and adaptive sampling, stop conditions, denoising | 13 |
| [12-output.md](12-output.md) | OUT | Output channels, light path expressions, Cryptomatte, deep data, image formats | 16 |
| [13-imager.md](13-imager.md) | IMGR | Imagers, light mixer and post-processing | 14 |
| [14-color.md](14-color.md) | COLOR | Color configuration, rendering space, display transforms | 10 |
| [15-render-view.md](15-render-view.md) | VIEW | Interactive render view, inspection, snapshots and comparison | 18 |
| [16-lookdev.md](16-lookdev.md) | LOOKDEV | Material editor and look-development stage | 13 |
| [17-overrides.md](17-overrides.md) | OVR | Looks, override layers, render passes and variants | 13 |
| [18-batch.md](18-batch.md) | BATCH | Command-line renderer, distributed rendering, render-farm worker | 21 |
| [19-host.md](19-host.md) | HOST | Host integration through the Arc Scene contract | 17 |
| [20-studies.md](20-studies.md) | STUDY | Daylight, illuminance, solar studies, panoramas and walkthroughs | 17 |
| [21-library.md](21-library.md) | LIB | Material, light and environment libraries | 12 |
| [22-diagnostics.md](22-diagnostics.md) | DIAG | Logging, statistics, reports, profiling and debug views | 12 |
| [23-extensibility.md](23-extensibility.md) | EXT | Automation, custom shading and sandboxed plug-ins | 12 |
| [24-interop.md](24-interop.md) | IO | OpenUSD, MaterialX, OpenPBR, OpenVDB, OpenEXR, other formats, content from other renderers and applications | 18 |
| [25-host-applications.md](25-host-applications.md) | HOSTAPP | Hydra render delegate, host connectors, render view inside other applications | 17 |
| [26-assistance.md](26-assistance.md) | ASSIST | Optional assistance: render time and noise explained, settings for a time budget, materials, lighting, cameras and looks from a description, studies, learned filter disclosure | 15 |
| | | **Total** | **455** |

## Capabilities and issues

- Every feature issue names the capability identifiers it implements in its *Scope* section and
  carries the `area: <token>` label of their file (lower case, for example `area: path`).
- An epic groups the capabilities of one milestone slice; [../../roadmap.md](../../roadmap.md)
  lists which identifiers each milestone covers.
- A change in behavior updates the row in the same pull request. A new behavior gets a new row with
  the next free number in its file.
- Acceptance criteria in issues are derived from the Behavior cell and from the
  [reference workflows](../workflows.md).
