# 0001. CPU reference integrator first, GPU integrator on wgpu held to parity

- Status: Accepted
- Capabilities: RB-PATH-015, RB-PATH-018, RB-PATH-019, RB-PATH-020, RB-PATH-021

## Context

Users need final-frame quality they can trust and GPU speed for interaction. Renderers that grew
separate CPU and GPU code paths commonly show look differences and undocumented feature gaps, which
forces users to verify every shot on both. GPU APIs also differ by vendor; a renderer bound to one
vendor's API excludes most hardware. The suite already standardizes on wgpu for real-time graphics.

## Decision

- The CPU integrator is built first and is the reference implementation of every feature. It is
  deterministic per platform (RB-PATH-015).
- The GPU integrator is built second on wgpu (Vulkan, Metal, Direct3D 12) and implements the same
  estimator: same sampling dimensions, same light hierarchy, same shading program.
- A feature is available on the GPU only after its scenes pass the statistical parity tests against
  the CPU. Each device publishes a feature matrix; unsupported features fail validation or fall back
  to the CPU by explicit setup policy, never silently.
- No vendor-specific GPU API is required for any feature.

## Consequences

- Phase 4 ships CPU-only; interactive speed on the CPU relies on progressive rendering,
  denoising and incremental updates.
- Every engine feature carries a parity corpus scene from the start, which is extra work but makes
  the GPU port measurable.
- Hardware ray tracing is used only where wgpu exposes ray queries; elsewhere compute traversal
  is used, which may be slower (OQ-13).

## Alternatives considered

- **GPU first.** Faster interaction earlier, but harder to debug, less portable and without a
  reference to test against.
- **Vendor-specific GPU APIs.** Mature ray tracing and denoising, but they exclude other vendors'
  hardware and conflict with the suite's vendor-neutral principle.
- **Separate GPU renderer with its own feature set.** Lower coupling, but produces the divergence this
  decision exists to prevent.

## Reopen when

wgpu cannot expose a capability required for production GPU rendering (for example ray queries or
sufficient buffer sizes) on a major platform for more than one suite release, or parity testing
shows a feature cannot converge to the CPU result on the GPU.
