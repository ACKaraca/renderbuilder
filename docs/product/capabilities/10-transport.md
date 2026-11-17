# Light transport and sampling

This area covers the integrator, light selection, path guiding, sampling patterns and determinism,
render devices and the spectral pipeline. The design is in
[../../architecture/integrator.md](../../architecture/integrator.md).

Domain objects: path, ray type, depth limits, light hierarchy, guiding field, sample sequence,
device, feature matrix.

## Integrator

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-001 | Unidirectional path tracing | Paths start at the camera and combine material sampling with next-event estimation through multiple importance sampling. The result is unbiased apart from clamps and depth limits, which are reported in metadata. | T1 | 4 |
| RB-PATH-002 | Depth limits | Separate limits for total, diffuse, specular, transmission, volume and transparency depth; paths beyond a minimum depth terminate by deterministic Russian roulette on throughput. | T1 | 4 |
| RB-PATH-003 | Transparency handling | Partial opacity is evaluated stochastically; beyond the transparency depth, remaining surfaces are treated as fully transparent so leaves and hair cards do not darken. | T1 | 4 |
| RB-PATH-004 | Caustic control | Caustic paths are enabled per light and per object, and an optional roughness regularization after diffuse bounces trades a small bias for much lower caustic noise. | T2 | 4 |
| RB-PATH-005 | Sample clamping | Direct and indirect sample luminance clamps limit fireflies; clamping can include or exclude outputs other than beauty. | T1 | 4 |

## Light sampling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-006 | Many-light sampling | A light hierarchy with spatial and orientation bounds selects lights by estimated contribution at each shading point, so cost stays nearly constant as light count grows. | T1 | 4 |
| RB-PATH-007 | Environment sampling | Environment and sky lights are importance sampled from their luminance and, with portals, through openings, combined with material sampling. | T1 | 4 |
| RB-PATH-008 | Textured emitter sampling | Textured area and mesh lights are sampled from their emission distribution, not only by area. | T2 | 4 |
| RB-PATH-009 | Light importance weighting | A per-light importance weight shifts samples toward or away from a light without changing its brightness. | T2 | 4 |

## Path guiding

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-010 | Path guiding | During progressive passes the renderer learns spatial and directional distributions of incident light and samples diffuse and glossy bounces from them in a mixture with material sampling. Interiors lit through small openings converge with fewer samples. | T3 | 5 |
| RB-PATH-011 | Guided light selection | Light selection learns from previous passes which lights actually reach a region, reducing samples spent on occluded lights. | T3 | 5 |
| RB-PATH-012 | Reproducible guiding | Guiding structures are trained at pass boundaries in a fixed order, so guided renders stay deterministic under RB-PATH-015. | T3 | 5 |

## Sampling and determinism

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-013 | Low-discrepancy sampling | Owen-scrambled Sobol sequences with per-pixel decorrelation and a fixed dimension allocation per bounce; at low sample counts the error is distributed as blue noise. | T1 | 4 |
| RB-PATH-014 | Seed control | The noise pattern changes per frame by default and can be fixed or offset per setup. | T1 | 4 |
| RB-PATH-015 | Deterministic output | The same scene, settings and RenderBuilder version produce bit-identical images on a given platform regardless of thread count, tile size, tile order or interruption and resume. | T1 | 4 |
| RB-PATH-016 | Motion blur sampling | Each camera ray samples a time in the shutter; transforms interpolate with rotation decomposition and deformations interpolate between samples, including multi-segment motion. | T1 | 4 |
| RB-PATH-017 | Invalid sample guard | Non-finite samples are discarded, counted and located by pixel in diagnostics instead of corrupting the image. | T1 | 4 |

## Devices

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-018 | CPU reference device | The CPU device supports every feature, uses all cores with vector instructions and is the reference other devices are measured against. | T1 | 4 |
| RB-PATH-019 | GPU device | The GPU device runs on Vulkan, Metal and Direct3D 12 adapters from any vendor through wgpu, using hardware ray queries when available and compute traversal otherwise. | T2 | 5 |
| RB-PATH-020 | Device feature matrix | Each device declares the shading program features it supports. A scene that uses an unsupported feature on the GPU fails validation or falls back to the CPU, per setup policy; it never renders a silently different image. | T2 | 5 |
| RB-PATH-021 | Parity report | A CLI command renders a scene on two devices and reports per-output statistical differences against the variance of each image, with pass or fail per the parity thresholds. | T2 | 5 |
| RB-PATH-022 | Multiple GPUs | One frame splits across several GPUs by samples or tiles with a deterministic merge. | T3 | 5 |
| RB-PATH-023 | Hybrid CPU and GPU | CPU and GPU devices contribute samples to the same frame with a deterministic merge. | T3 | 5 |

## Spectral rendering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-PATH-024 | Spectral mode | A spectral mode carries wavelength samples along paths, upsamples RGB inputs to spectra and renders dispersion, thin film and colored glass per wavelength. RGB and spectral modes share one scene and one color pipeline. | T3 | 5 |
| RB-PATH-025 | Spectral inputs | Lights accept spectral power distributions (standard illuminants or measured data), and dielectrics accept index of refraction as a function of wavelength. | T3 | 5 |
