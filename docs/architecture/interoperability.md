# Interoperability

This document lists the formats RenderBuilder reads and writes, at which fidelity level, how
content is mapped, how exchange is validated and losses reported, and what is deliberately
unsupported. Fidelity levels follow suite ADR-0009: **view** (geometry and appearance), **data**
(semantics, properties, relationships) and **edit** (native editing).

## Formats

| Format | Direction | Fidelity | Notes | Capabilities |
|---|---|---|---|---|
| Arc Scene package and live session | Read | Edit | The native contract; materials, lights and cameras are editable through looks and overrides; host semantics arrive as attributes. | RB-IO-003, RB-HOST-001 |
| OpenUSD (`.usd`, `.usda`, `.usdc`, `.usdz`) | Read | Data | Geometry, lights, cameras, materials, render settings, variants, instancing, time samples; payloads become stand-ins. Materials become editable networks (edit) when expressed in MaterialX or UsdPreviewSurface. | RB-IO-001 |
| OpenUSD | Write | Data | Flattened resolved render scene with RenderBuilder attributes in a documented namespace; used by render packages. | RB-IO-002, RB-SCENE-005 |
| MaterialX (`.mtlx`) | Read and write | Edit | Node graphs, node definitions, OpenPBR and standard libraries, looks and material assignments. | RB-IO-008, RB-OVR-004 |
| OpenPBR | Read and write (through MaterialX and OpenUSD) | Edit | Exact parameter semantics of the recorded specification version. | RB-SURF-001, RB-IO-009 |
| OSL compiled shaders (`.oso`) | Read | Edit (parameters) | Translated into the shading program within a documented subset. | RB-NODE-022, RB-EXT-006 |
| glTF 2.0 (`.gltf`, `.glb`) | Read | View | Meshes, transforms, cameras, punctual lights, metallic-roughness materials and supported material extensions. | RB-IO-004 |
| PLY | Read | View | Meshes, point clouds, Gaussian splat sets. | RB-IO-005, RB-GEOM-015 |
| OBJ | Read | View | Meshes, groups, basic materials. | RB-IO-006 |
| Alembic (`.abc`) | Read | View | Meshes, curves, points, transforms, time samples. | RB-IO-007 |
| OpenVDB (`.vdb`), NanoVDB (`.nvdb`) | Read | View | Several grids, float, half and vector. Level-of-detail grids written by the CLI. | RB-IO-011, RB-VOL-002 |
| OpenEXR | Read | View | Textures, environments, plates; scanline, tiled, mip-mapped, multipart. | RB-IO-012 |
| OpenEXR | Write | Data | Multilayer, multipart, multi-view, deep; full metadata and Cryptomatte manifests. | RB-OUT-012, RB-OUT-010 |
| TIFF, PNG, JPEG, Radiance HDR | Read and write | View | Textures and display-referred outputs. | RB-TEX-004, RB-OUT-013 |
| IES LM-63, EULUMDAT | Read | Data | Photometric distributions with orientation and flux. | RB-IO-013 |
| OpenColorIO configuration | Read | Data | Color spaces, displays, views, looks, file rules. | RB-COLOR-001 |
| Cryptomatte 1.2 | Write | Data | In OpenEXR. | RB-OUT-009 |
| Hydra scene data | Read | Data | Scene delivered by a Hydra render delegate running in a host. | RB-HOSTAPP-003 |
| Host application scene | Read | Data | Translated by a connector through the host's public API; edit fidelity of materials where the host exposes definitions. | RB-HOSTAPP-007, RB-HOSTAPP-008 |
| Other renderers' scene and material formats with a published specification or an open reader | Read | View, data for materials | Native readers behind `arc-io-*` crates, with loss reports. | RB-IO-016, RB-IO-017 |
| Render package | Read and write | Edit | Manifest, flattened OpenUSD, MaterialX, content-addressed assets. | RB-SCENE-005 |
| Library package | Read and write | Edit | MaterialX documents, textures, thumbnails, JSON manifest. | RB-LIB-002 |
| Study results (CSV, JSON) | Write | Data | Sensor values, statistics, legend parameters. | RB-STUDY-008 |
| Panorama tour description (JSON) | Write | Data | Panorama images, positions and links for Arc Review and the Arc Hub viewer. | RB-STUDY-015 |

## Mapping principles

1. `arc-io-*` crates produce neutral models of each format; `renderbuilder-io` owns the mapping
   into the render scene.
2. Source identity is preserved: OpenUSD paths and Arc identifiers become stable prim identities,
   and Cryptomatte names use them.
3. Materials are never approximated silently. A source material is either rendered from its own
   definition (UsdPreviewSurface, glTF, MaterialX, OpenPBR) or replaced by the diagnostic material
   with a loss entry.
4. Units convert at the boundary: scene lengths to meters (suite ADR-0008), light quantities to the
   document's photometric units, colors to the rendering space.
5. Unknown prims and attributes are preserved in the scene graph and listed; they are not dropped
   from render packages.
6. Exports write only standard schemas plus attributes in the documented RenderBuilder namespace,
   so other tools ignore what they do not understand.

## Validation

- Every exporter is validated in continuous integration by the format's independent tooling:
  `usdchecker` for OpenUSD, the MaterialX document validator, and the OpenEXR reference reader and
  `exrcheck` for images (RB-IO-014).
- Imports are validated by reference renders on the interchange corpus; each corpus file has an
  owner, license and checksum row in its corpus repository.
- OpenPBR conformance compares renders of the specification's reference scenes (RB-IO-009).
- Cryptomatte output is checked by decoding mattes with an independent implementation.

## Loss reporting

Every import produces a loss report (RB-IO-015, RB-IO-010) with one entry per unsupported prim,
attribute, node or parameter: source path, what was lost, what replaced it (nothing, a placeholder,
a default) and whether the item is preserved for re-export. Reports are available in the UI, as JSON
from the CLI and in render statistics. Batch renders can be configured to fail when the loss report
is not empty.

## Exchange through connectors

Content whose format is neither published nor openly readable is exchanged by a connector running
inside the user's own licensed copy of the application that owns it, through that application's
public API (suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md)).
The connector sends an Arc Scene snapshot to a session and receives images (RB-HOSTAPP-006,
RB-IO-018). Hosts that implement the Hydra rendering interface use the render delegate instead
(RB-HOSTAPP-001). Every translation produces a loss report (RB-HOSTAPP-016).

## Deliberately unsupported

Reverse engineering undocumented formats, and running compiled or protected scripts and shaders of
other applications, are excluded because they create a legal problem; the content is obtained
through a connector or a standard format instead.

| Content | Reason | Exchange instead |
|---|---|---|
| Native files of other renderers and applications without a published specification or open reader | Not published | A connector inside the owning application, or OpenUSD and MaterialX exports |
| FBX | Read through the open reader in `arc-io` when a host sends FBX content; not a native scene source | OpenUSD or glTF preferred |
| Compiled shader binaries of other renderers | Not portable | OSL source compiled with the OSL compiler, or MaterialX |
