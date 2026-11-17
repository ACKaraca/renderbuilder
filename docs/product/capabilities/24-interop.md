# Interoperability

This area covers the open scene, material, volume and image standards RenderBuilder reads and
writes. Fidelity levels and mapping rules are in
[../../architecture/interoperability.md](../../architecture/interoperability.md).

Domain objects: import, export, mapping, loss report.

## Scene formats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-IO-001 | OpenUSD read | Reads meshes, basis curves, points, point instancers, transforms, cameras, UsdLux lights, UsdShade materials (UsdPreviewSurface and MaterialX), render settings, products and variables, variants, instancing and time samples; payloads become stand-ins. | T1 | 4 |
| RB-IO-002 | OpenUSD write | Writes the resolved render scene as a flattened stage, used by render packages and for hand-off to other tools. | T2 | 4 |
| RB-IO-003 | Arc Scene packages | Reads Arc Scene packages exported by Arc products with full fidelity. | T1 | 4 |
| RB-IO-004 | glTF read | Reads glTF 2.0 meshes, transforms, cameras, lights and metallic-roughness materials with supported material extensions. | T2 | 4 |
| RB-IO-005 | PLY read | Reads PLY meshes, point clouds and Gaussian splat sets. | T2 | 4 |
| RB-IO-006 | OBJ read | Reads OBJ meshes with groups and basic materials. | T2 | 4 |
| RB-IO-007 | Alembic read | Reads Alembic meshes, curves, points, transforms and time samples as stand-ins or sources. | T2 | 5 |

## Materials

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-IO-008 | MaterialX read and write | Reads and writes MaterialX documents with the standard, physically based and OpenPBR node definitions and looks, at the MaterialX version recorded in the document. | T1 | 4 |
| RB-IO-009 | OpenPBR conformance | OpenPBR renders are compared with the specification's reference scenes, and deviations beyond tolerance are treated as defects. | T1 | 4 |
| RB-IO-010 | Material loss reports | Unsupported nodes, parameters and shading models found on import are listed per material, and placeholders mark them in the render. | T1 | 4 |

## Volumes, images and data

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-IO-011 | OpenVDB and NanoVDB read | Reads `.vdb` files with several grids of float, half and vector type, and `.nvdb` files. | T1 | 4 |
| RB-IO-012 | OpenEXR read | Reads scanline, tiled, mip-mapped and multipart OpenEXR images for textures, environments and plates. | T1 | 4 |
| RB-IO-013 | Photometric files | Reads IES LM-63 and EULUMDAT files. | T1 | 4 |

## Validation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-IO-014 | Independent validation | Written OpenUSD, MaterialX and OpenEXR files pass the independent validators of those projects in continuous integration. | T1 | 4 |
| RB-IO-015 | Import loss reports | Every import produces a loss report listing unsupported prims, attributes and nodes with their source paths. | T1 | 4 |
