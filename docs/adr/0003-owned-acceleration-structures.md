# 0003. Acceleration structures owned in Rust

- Status: Accepted
- Capabilities: RB-GEOM-019, RB-GEOM-020, RB-GEOM-028, RB-PATH-015, RB-PATH-019

## Context

Ray traversal speed dominates render time. Mature C++ kernels such as Embree offer excellent CPU
performance. RenderBuilder also needs determinism, a traversal layout the GPU device can share,
custom primitives (curves, splats, analytic shapes, motion blur) with identical semantics on both
devices, and as little `unsafe` code as possible (suite ADR-0015).

## Decision

- Two-level bounding volume hierarchies (instances over prototypes) are built and traversed by
  RenderBuilder's own Rust code, with a compact node layout shared by the CPU and GPU devices.
- Construction uses binned surface-area heuristics in parallel with deterministic ordering;
  refitting serves transform-only changes.
- Intersection routines for triangles, curves, points, splats and analytic primitives use
  watertight formulations with error bounds that also drive ray offsets (RB-GEOM-028).
- On GPUs with hardware ray queries, the device builds hardware structures from the same
  primitives; compute traversal of the shared layout is the fallback.
- Embree is not linked.

## Consequences

- Full control over determinism, memory layout and primitive semantics; no native dependency in
  the CPU path.
- CPU traversal must be optimized with SIMD to stay competitive; this is engineering work the
  project owns.
- The traversal code is a significant subsystem that needs its own benchmark corpus.

## Alternatives considered

- **Embree through an FFI crate.** Fastest path to high CPU performance and permissively licensed,
  but adds a large native dependency, separate primitive semantics from the GPU device and less
  control over determinism.
- **Existing Rust BVH crates.** Useful references, but none covers motion blur, curves, two-level
  instancing and a GPU-shared layout together.

## Reopen when

On the traversal benchmark corpus, CPU ray throughput stays below 70 percent of a reference
native kernel after SIMD optimization for two consecutive releases.
