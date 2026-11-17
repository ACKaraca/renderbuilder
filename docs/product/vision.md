# Product vision

RenderBuilder is the physically based production renderer and look-development tool of the Arc
suite. This document states the problem it solves, for whom, the principles it follows and what it
deliberately does not do.

## Problem and purpose

Images are how buildings, products, films and games are judged before they exist. Teams that author
models in one application and render in another lose time to conversion, to materials that do not
survive the trip, to renderers tied to one GPU vendor, and to licensing that follows every render
node. Architects need daylight and illuminance answers from the same model that produces their
drawings; look-development artists need predictable, reproducible images; pipeline engineers need a
renderer they can script, package and scale without a license server.

RenderBuilder provides one renderer for all of these: a deterministic, physically based path tracer
driven by open scene and material standards, usable interactively inside every Arc product,
standalone for look development, and headless on a render farm.

## Primary users and deliverables

| User | Deliverables |
|---|---|
| Architectural visualization artists and architects | Interior and exterior stills, panoramas and tours, walkthrough sequences, clay renders, presentation images placed on sheets |
| Lighting and daylight designers | Illuminance and luminance studies, daylight factor grids, solar studies, sun-hour maps, study reports |
| Product designers and engineers | Product stills, turntables, color and material variants, exploded-view animations |
| Look-development and lighting artists | Material libraries, look files (MaterialX), turntables, lit shots with output channels for compositing |
| Animation and visual-effects artists | Image sequences with output channels, light path expressions, Cryptomatte and deep data |
| Pipeline engineers and render wranglers | Render packages, farm jobs, automation scripts, statistics and reports |

## Product principles

1. **Physically based by default, artistic by choice.** Defaults produce physically plausible
   images in real units; non-physical controls exist, are explicit and are flagged in output
   metadata.
2. **Deterministic and reproducible.** The same inputs and version produce the same image on a
   given platform, regardless of threads, tiles or interruptions.
3. **One engine, many entry points.** The desktop application, host viewports, the command line and
   farm workers drive the same engine through the same session interface.
4. **Open formats only.** OpenUSD, MaterialX, OpenPBR, OSL, OpenVDB, OpenEXR and OpenColorIO are the
   working vocabulary; nothing requires a proprietary format.
5. **The scene source stays authoritative.** Looks and overrides layer over the scene and never
   modify the host model.
6. **The CPU integrator is the reference.** Every other device is measured against it and may not
   diverge silently.
7. **No license server, no metering.** Every machine renders with the same features; farm rendering
   needs no per-node license or credit.

## Scope

The capability files in [capabilities/](capabilities/README.md) define the scope: scene description,
cameras, geometry, volumes, lights, surface, hair and stylized shading, shading networks, textures,
light transport, adaptive sampling and denoising, outputs, imagers, color management, the
interactive render view, the material editor and look-development stage, looks and overrides,
batch and farm rendering, host integration, architectural studies, libraries, diagnostics,
extensibility and interoperability.

## Non-goals

| Non-goal | Reason |
|---|---|
| Modeling, sculpting, rigging and animation authoring | They belong to MeshBuilder and the other authoring products; RenderBuilder only edits what rendering needs (looks, lights, cameras, setups). |
| Real-time rasterized rendering as a final-image engine | The real-time viewport is `arc-gpu` in every product; RenderBuilder adds progressive path tracing on top of it, not a second rasterizer. |
| Plug-ins for third-party content-creation applications | Third-party tools exchange scenes through OpenUSD and MaterialX; maintaining plug-ins for other vendors' hosts would tie releases to their schedules. |
| Reading other renderers' proprietary scene files and material libraries | They lack published specifications; such content is exchanged through OpenUSD, MaterialX and glTF (ADR-0009 of the suite). |
| Bidirectional, photon-mapping and cache-based global illumination | One unbiased unidirectional integrator with path guiding keeps behavior predictable and the CPU and GPU implementations at parity. |
| Licensing, activation, watermarks and usage metering | The suite is open source and account free; nothing restricts output. |
| A hosted cloud render service operated by the project | Farm rendering is self-hosted through Arc Hub; operating a paid service is outside the open-source repositories. |
| Analytic NURBS rendering | Hosts tessellate exact geometry to the requested tolerance before sending it; a second exact-surface path would duplicate `arc-brep`. |
| Embedded script interpreters | Scripting runs out of process through the automation protocol, as everywhere in the suite. |

## Positioning

RenderBuilder covers the professional scope served today by production renderers such as Autodesk
Arnold, Chaos V-Ray, Pixar RenderMan, Maxon Redshift and Blender Cycles, and by the visualization
and daylight tools built into building-design applications.

## Differentiators

| Capability | Where specified |
|---|---|
| GPU rendering on any vendor's hardware through wgpu, held to parity with the CPU reference by image-comparison tests and a declared feature matrix | RB-PATH-019, RB-PATH-020, RB-PATH-021 |
| Path guiding and many-light sampling for interiors and scenes with thousands of fixtures | RB-PATH-006, RB-PATH-010, RB-PATH-011 |
| Bit-identical deterministic output across thread counts, tiles, resumes and distributed merges | RB-PATH-015, RB-NOISE-007, RB-BATCH-012, RB-BATCH-013 |
| Spectral-ready color pipeline with an optional spectral mode | RB-COLOR-010, RB-PATH-024, RB-PATH-025 |
| Open scene and material formats only, with loss reports on every import | RB-IO-001, RB-IO-008, RB-IO-015 |
| Self-hosted render farm through Arc Hub with no credits, per-node licenses or metering | RB-BATCH-015, RB-BATCH-021 |
| Live, crash-isolated rendering inside every Arc product, from real-time viewport to path-traced final | RB-HOST-001, RB-HOST-005, RB-HOST-008 |
| Host semantics (IFC classes, categories, levels, systems) usable directly in look rules | RB-HOST-015, RB-SCENE-017 |
| The same engine for stills, animation, panoramas, illuminance and solar studies, with a physically valid mode and validated results | RB-STUDY-005, RB-STUDY-007, RB-STUDY-010, RB-STUDY-012 |
| Automatic, content-addressed texture conversion shared across renders and farm workers | RB-TEX-009, RB-TEX-011 |
| Building-material appearance templates mapped to OpenPBR with real-world scale | RB-LIB-007, RB-TEX-007 |
| Temporal denoising and component-consistent denoising for flicker-free sequences | RB-NOISE-009, RB-NOISE-011 |
| Render packages that reproduce a render on any machine and serve as bug reports | RB-SCENE-005, RB-BATCH-006 |
