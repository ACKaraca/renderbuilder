# Outputs

This area covers render outputs: built-in and custom output channels, light path expressions,
Cryptomatte, deep images, pixel filters, image formats, metadata and file writing.

Domain objects: render output, output set, light path expression, label, pixel filter, file
template, image file.

## Output definitions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OUT-001 | Render outputs | Each output names a source channel, data type (RGBA, RGB, float, vector, identifier), pixel filter, file template and layer or part name; a setup holds any number of outputs written to one or more files. | T1 | 4 |
| RB-OUT-002 | Built-in channels | Beauty; direct and indirect diffuse, specular, transmission, subsurface, coat, fuzz and volume; emission; background; albedo; shadow; position; normal; reference position; depth; motion vectors; UV; object and material identifiers; opacity; samples per pixel; render time per pixel. | T1 | 4 |
| RB-OUT-003 | Beauty reconstruction | The lighting component channels sum to beauty; any documented difference from clamping or filtering is listed with the channel definitions. | T1 | 4 |
| RB-OUT-004 | Light path expressions | Outputs can be defined by regular expressions over scattering events (camera, reflection and transmission, diffuse, glossy and specular, volume, subsurface, emission, light, background and labels). Invalid expressions are rejected with the error position. | T1 | 4 |
| RB-OUT-005 | Labels | Lights, materials and objects carry labels that light path expressions reference, for per-object or per-material lighting splits. | T2 | 4 |
| RB-OUT-006 | Light group outputs | Each light group writes beauty and, on request, per-lobe channels; the groups plus emission and background sum to beauty. | T1 | 4 |
| RB-OUT-007 | Pixel filters | Box, triangle, Gaussian, Blackman-Harris and Mitchell-Netravali filters with width per output. Data channels such as depth, position and identifiers use closest-sample, minimum or maximum filtering instead of averaging. | T1 | 4 |
| RB-OUT-008 | Alpha and transparent background | Color outputs carry premultiplied alpha; the background can render transparent while still lighting the scene. | T1 | 4 |

## Cryptomatte

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OUT-009 | Cryptomatte | Object, material and asset layers with configurable rank depth and preview channels follow the Cryptomatte 1.2 specification. Names come from stable prim paths, the manifest is stored in metadata, and instances can be split into separate names. | T1 | 4 |

## Deep images

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OUT-010 | Deep output | Each pixel stores a list of samples with front and back depth, alpha, color and chosen channels, merged by depth and alpha tolerance, written as deep OpenEXR. | T2 | 5 |
| RB-OUT-011 | Deep volumes | Volume contributions are stored as deep segments with correct per-segment alpha. | T3 | 5 |

## Image formats and files

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OUT-012 | OpenEXR output | Half and float channels, scanline or tiled, compression none, RLE, ZIP, ZIPS, PIZ, PXR24, B44, DWAA or DWAB, multilayer and multipart files with per-part compression, data and display windows and multi-view stereo. | T1 | 4 |
| RB-OUT-013 | Display formats | PNG, JPEG, TIFF (8, 16 and 32-bit float) and Radiance HDR. Display-referred formats apply the view transform and imagers; 8-bit files are dithered. | T1 | 4 |
| RB-OUT-014 | Metadata | Files carry camera matrices, a settings digest, document and setup identifiers, RenderBuilder version, render time, color spaces, sample statistics, stop condition, Cryptomatte manifests and user key-value pairs. | T1 | 4 |
| RB-OUT-015 | File templates | File paths use tokens for setup, camera, output, frame with padding, eye and tile; an overwrite policy chooses replace, version-increment or skip. | T1 | 4 |
| RB-OUT-016 | Safe writing | Frames are written to a temporary file and renamed atomically when complete; incomplete frames are never left under the final name. | T1 | 4 |
