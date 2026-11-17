# Adaptive sampling and denoising

This area covers progressive and adaptive sampling, stop conditions, checkpoints, and denoising of
stills, interactive frames and sequences.

Domain objects: pass, convergence estimate, stop condition, checkpoint, denoiser, auxiliary
buffer.

## Progressive and adaptive sampling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NOISE-001 | Progressive rendering | The image refines in passes of increasing samples per pixel over the whole frame; the first interactive pass may render at reduced resolution and is replaced as soon as full-resolution samples arrive. | T1 | 4 |
| RB-NOISE-002 | Adaptive sampling | A per-pixel error estimate stops pixels below a threshold between minimum and maximum sample counts. Error is measured after the view transform, on beauty or on a chosen output, so the threshold matches visible noise. | T1 | 4 |
| RB-NOISE-003 | Stop conditions | A render stops at the first of maximum samples, error threshold, time limit or a user stop, and records which condition ended it in metadata. | T1 | 4 |
| RB-NOISE-004 | Frame time limit | A per-frame wall-clock limit returns the best image reached and flags it as time-limited; the limit is ignored in interactive rendering. | T2 | 4 |
| RB-NOISE-005 | Tiled rendering | Final renders can proceed in tiles of configurable size and order (spiral, Hilbert, scanline) to bound memory and show progress; tiling never changes the result. | T2 | 4 |
| RB-NOISE-006 | Convergence readout | The render view and statistics show samples per pixel and the remaining error per pass, with an estimate of time to the threshold. | T2 | 4 |
| RB-NOISE-007 | Checkpoint and resume | Accumulated sample state saves to a checkpoint file; resuming adds samples and produces the same image as an uninterrupted render. | T2 | 4 |

## Denoising

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NOISE-008 | Denoiser | Open Image Denoise removes noise from high-dynamic-range images using albedo and normal auxiliaries, prefiltered for clean feature buffers, on the CPU and on supported GPUs. | T1 | 4 |
| RB-NOISE-009 | Component denoising | Beauty components and light groups denoise individually so their sum stays consistent with the denoised beauty in compositing. | T2 | 4 |
| RB-NOISE-010 | Interactive denoising | The render view denoises progressive frames at a set cadence and shows denoised and raw images on request. | T1 | 4 |
| RB-NOISE-011 | Temporal denoising | A CLI command denoises OpenEXR sequences using motion vectors and neighboring frames, removing frame-to-frame flicker. | T2 | 5 |
| RB-NOISE-012 | Denoising auxiliaries | Albedo, normal, depth, motion vectors and per-pixel variance can be written as outputs for external denoisers. | T1 | 4 |
| RB-NOISE-013 | Reduced-resolution interaction | During camera navigation the render view renders at reduced resolution and reconstructs full resolution with a denoise and upsample pass that runs on any GPU vendor. | T2 | 5 |
