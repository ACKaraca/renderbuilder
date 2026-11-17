# Scene description and scene graph

This area defines what RenderBuilder renders: the render document, the scene sources it composes,
the path-addressed scene graph, per-object render attributes, collections, asset resolution and the
render setups that turn a scene into images.

Domain objects: render document, scene source, render scene, prim, attribute, collection,
selector expression, render setup, render package.

## Render document and scene sources

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-001 | Render document | A `.renderb` document stores scene source references, render setups, looks, override layers, light rigs, local materials, studies and snapshots. Scene geometry is referenced, never copied into the document; saves are atomic and every change is an undoable command. | T1 | 4 |
| RB-SCENE-002 | Scene sources | A document references one or more sources: an Arc Scene package file, a live host session, an OpenUSD stage, or a render package. Each source records its location, content digest and resolved revision; a missing source is reported and the document still opens with the source shown as unresolved. | T1 | 4 |
| RB-SCENE-003 | Source composition | Several sources compose into one render scene in a stated order; later sources add prims and, on path collisions, win. Every collision is listed in the composition report. | T2 | 4 |
| RB-SCENE-004 | Source reload | A changed source digest is detected on open and on request; reloading applies the difference as a scene delta and keeps looks, overrides and links keyed by stable identity. Assignments whose targets no longer exist are listed as orphaned instead of being deleted. | T1 | 4 |
| RB-SCENE-005 | Render package | A render package is a folder or single-file archive with a manifest, a flattened OpenUSD stage carrying RenderBuilder attributes, MaterialX documents and content-addressed assets. It renders identically on any machine with the same RenderBuilder version and is the unit of farm submission and bug reports. | T1 | 4 |

## Scene graph

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-006 | Hierarchy and transforms | Prims form a path-addressed hierarchy; each prim has a local 4x4 transform composed in f64. Visibility and render attributes inherit down the hierarchy unless a prim sets its own value. | T1 | 4 |
| RB-SCENE-007 | Stable identity | Every prim carries a stable identifier taken from its source (Arc object ID or OpenUSD path). Looks, overrides, Cryptomatte names, light links and incremental updates refer to this identifier, so they survive reloads and reordering. | T1 | 4 |
| RB-SCENE-008 | Prim kinds | The scene graph holds polygon meshes, subdivision meshes, curves, points, volumes, Gaussian splat sets, stand-ins, instancers, lights and cameras. Prims of kinds the renderer does not know are preserved in the scene, listed in the validation report and not rendered. | T1 | 4 |
| RB-SCENE-009 | Primvars and attributes | Prims carry typed attributes (bool, int, float, vector, point, normal, color, matrix, string and arrays) with constant, per-face, per-vertex, per-face-vertex or per-instance interpolation. Shading networks read them by name with a typed fallback. | T1 | 4 |
| RB-SCENE-010 | Motion samples | Transforms, point positions, normals and animatable parameters accept one or more time samples at arbitrary times. The shutter interval selects the samples used; velocity and acceleration attributes are an accepted substitute for multiple position samples. | T1 | 4 |
| RB-SCENE-011 | Time and frames | The document defines frame rate and frame range; animated sources are evaluated at frame and subframe times, including the shutter samples around each frame. | T1 | 4 |

## Object render attributes

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-012 | Ray visibility | Each prim has visibility flags for camera, shadow, diffuse reflection, specular reflection, diffuse transmission, specular transmission, volume scattering and subsurface rays. A prim hidden from camera still appears in the ray types left enabled. | T1 | 4 |
| RB-SCENE-013 | Sidedness and opacity hint | A prim can be one- or two-sided per ray type, and an opaque hint skips opacity evaluation on its materials. Turning the hint on for a prim whose material has opacity below one emits a validation warning. | T2 | 4 |
| RB-SCENE-014 | Holdout | A holdout prim renders zero color and zero alpha in beauty and all color outputs while still occluding other prims. Its shadow and reflection participation follow its visibility flags. | T1 | 4 |
| RB-SCENE-015 | Shadow participation | Per-prim flags control receiving shadows and self-shadowing. Disabling self-shadowing removes shadows the prim casts on itself without affecting shadows on other prims. | T2 | 4 |

## Collections and selection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-016 | Collections | A collection is a named set of prims defined by explicit membership, a selector expression or both. Expression collections are re-evaluated when the scene changes, and membership can be listed at any time. | T1 | 4 |
| RB-SCENE-017 | Selector expressions | Selector expressions match prims by path pattern (`*` one level, `**` any depth), kind, attribute predicates (`[@category == "window"]`), material name and collection membership, combined with and, or and not. Invalid expressions are rejected with the error position; the same language is used by looks, overrides and light linking. | T1 | 4 |

## Asset resolution and dependencies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-018 | Asset resolution | Asset paths resolve relative to the referencing source, then through document and user search paths, with token substitution for `<udim>`, `<uvtile>`, `<frame>` and `<attr:name>`. Each resolution keeps a trace of the locations tried. | T1 | 4 |
| RB-SCENE-019 | Dependency listing | The renderer lists every external file a document or render needs (textures, volumes, stand-ins, shader sources, photometric profiles, color configuration, plug-ins) with resolved path, size, digest and missing status, as a table and as JSON. Packaging and farm submission use this list. | T1 | 4 |
| RB-SCENE-020 | Missing asset policy | Each render setup chooses how missing assets are handled: fail the render, substitute a placeholder (a flagged texture color or a bounding box) and warn, or skip the asset. The log ends with a summary of every substitution. | T1 | 4 |

## Render setups

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SCENE-021 | Render setup | A render setup combines camera, resolution and pixel aspect, frame range, look, override layers, sampling and transport settings, outputs, imagers, color configuration and device. A document holds any number of setups; one is active in the render view. | T1 | 4 |
| RB-SCENE-022 | Setup presets | Built-in presets (preview, standard, final) and user presets stored in the document or user library fill a chosen subset of setup fields. Applying a preset shows the fields it changes before it is committed. | T2 | 4 |
| RB-SCENE-023 | Render region | A setup can restrict rendering to a pixel rectangle. Output files keep the full display window and store the region as the data window. | T1 | 4 |
| RB-SCENE-024 | Scene validation | Before rendering, validation checks for a missing camera, missing assets, degenerate geometry, invalid shading networks, empty collections in rules and features the selected device does not support. Findings carry a severity and a prim path; batch renders can be set to stop on errors. | T1 | 4 |
| RB-SCENE-025 | Camera background | The camera-visible background is set independently of environment lighting: a color, a vertical gradient, a screen-space image plate, or the environment itself. Changing the background does not change illumination. | T1 | 4 |
