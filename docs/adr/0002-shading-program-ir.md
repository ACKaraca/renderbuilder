# 0002. One shading program representation for all devices

- Status: Accepted
- Capabilities: RB-NODE-001, RB-NODE-005, RB-NODE-022, RB-SURF-001, RB-EXT-005, RB-EXT-006

## Context

Materials arrive as MaterialX graphs, OpenPBR parameter sets, UsdPreviewSurface and glTF materials,
and OSL shaders. They must render identically on the CPU and the GPU. Embedding a compiler
infrastructure such as LLVM adds a large native dependency and a JIT that the GPU cannot use.
WebAssembly plug-ins cannot run inside GPU shading.

## Decision

- All materials compile into one typed intermediate representation in static single-assignment
  form, the shading program, with closures (the OpenPBR lobe set, hair, volume and toon closures)
  as first-class outputs.
- MaterialX node graphs translate node by node; node definitions with node graph implementations
  extend the library without code (RB-EXT-005).
- OSL support translates compiled `.oso` shaders into the shading program within a documented
  subset (RB-NODE-022); the OSL compiler is an external tool and no OSL runtime is linked.
- The CPU executes the program with a vectorized interpreter over batches of shading points; the
  GPU device translates the program to WGSL.
- Custom shading code is not accepted as WebAssembly; plug-ins extend CPU-side stages only.

## Consequences

- One place defines shading semantics, which keeps devices at parity.
- OSL shaders that rely on features outside the subset do not render; they are reported.
- Interpreter performance must be engineered (batching, specialization of constant parameters);
  a native code generator for the CPU can be added behind the same IR later.

## Alternatives considered

- **Embedding OSL with its LLVM JIT.** Full OSL compatibility on the CPU, but a heavy native
  dependency, no GPU path through wgpu, and two shading semantics.
- **Hand-written closures per device.** Simple at first, but every material model would be
  implemented twice.
- **WGSL as the single language, run on the CPU by emulation.** Ties CPU performance to a
  shading language designed for GPUs.

## Reopen when

The interpreter cannot meet the interactive targets in the architecture overview on the shading
corpus after specialization, or the OSL subset blocks a majority of shaders in the open-source
corpus surveyed under OQ-11.
