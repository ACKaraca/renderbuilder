# Textures and the texture cache

This area covers image textures, the tiled mip-mapped texture cache and its automatic conversion,
procedural textures and texture baking.

Domain objects: image texture, texture tile, mip level, texture cache, cache entry, procedural
pattern.

## Image textures

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-TEX-001 | Image texture node | Reads an image by path with tokens, UV set, wrap mode (periodic, clamp, black, mirror), filter (closest, bilinear, bicubic), mip bias, missing-texture color and color space. | T1 | 4 |
| RB-TEX-002 | UDIM tiles | `<udim>` and `<uvtile>` tokens select tiles by UV range; missing tiles return the fallback color instead of failing. | T1 | 4 |
| RB-TEX-003 | Attribute tokens | `<attr:name>` tokens choose texture files per object from an attribute, so one material textures many objects differently. | T2 | 4 |
| RB-TEX-004 | Image formats | OpenEXR (scanline, tiled, mip-mapped, multipart), TIFF, PNG, JPEG and Radiance HDR textures are read at 8, 16 and 32 bits per channel. | T1 | 4 |
| RB-TEX-005 | Footprint filtering | Ray differentials carry texture footprints through reflection and refraction; anisotropic filtering selects mip levels so distant textures are neither blurred nor aliased. | T1 | 4 |
| RB-TEX-006 | Texture sequences | `<frame>` tokens animate textures with an offset and hold behavior outside the available range. | T2 | 4 |
| RB-TEX-007 | Real-world mapping | Textures can be sized in meters with offset and rotation in world or object space, independent of UV layout, as building materials specify. | T1 | 4 |

## Texture cache

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-TEX-008 | Tiled mip-mapped cache | Texture tiles load on demand at the mip level a lookup needs and are evicted least-recently-used under a memory budget. File handles are pooled under an open-file limit, and statistics are kept per file. | T1 | 4 |
| RB-TEX-009 | Automatic conversion | Untiled or unmipped textures convert in the background to the cache format, keyed by source digest, color space and conversion settings in a local content-addressed cache, and are reused by later renders. The render uses the source image until conversion finishes. | T1 | 4 |
| RB-TEX-010 | Batch conversion | A CLI command converts texture sets ahead of time with color conversion, filter and tile size options and reports the entries created. | T1 | 4 |
| RB-TEX-011 | Shared cache | Several farm workers share one cache directory; entries are written atomically, so concurrent workers never read partial files. | T2 | 4 |
| RB-TEX-012 | GPU texture streaming | On the GPU device, texture tiles stream into a fixed-size device pool so scenes whose textures exceed device memory still render. | T2 | 5 |

## Procedural textures

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-TEX-013 | Noise | Gradient and fractal noise in 2D, 3D and 4D (with time) with octaves, lacunarity, gain and distortion. | T1 | 4 |
| RB-TEX-014 | Cellular patterns | Cellular noise with distance metrics, first and second feature distances, randomness and per-cell color. | T1 | 4 |
| RB-TEX-015 | Geometric patterns | Checker, grid and brick or tile patterns with mortar width, bond offsets and per-tile color and texture randomization. | T1 | 4 |
| RB-TEX-016 | Flake normals | A flake pattern produces per-flake normals and masks for paints and glitter. | T2 | 4 |

## Baking

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-TEX-017 | Texture baking | Any network output, lighting component or output channel bakes into UV textures per object, UDIM aware, with padding and resolution per object. | T2 | 5 |
