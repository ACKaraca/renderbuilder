# Roadmap

RenderBuilder milestones mapped onto the suite phases. Milestones have exit criteria, not dates.
RenderBuilder starts in suite phase 4 (visualization and content); GPU path tracing and production
depth follow in phase 5. Scope is given as capability identifiers; "phase-4 T1 rows of X" means
every row of file X with tier T1 and phase 4.

## Overview

| Milestone | Suite phase | Goal |
|---|---|---|
| M1 Reference renderer | 4 | A deterministic CPU path tracer that renders open-standard scenes from the command line. |
| M2 Look development and interactive rendering | 4 | The desktop application: render view, material editor, looks, imagers, libraries and professional rendering features. |
| M3 Hosts, studies and farm | 4 | Rendering inside Arc products, architectural studies and the self-hosted farm. |
| M4 GPU device | 5 | Vendor-neutral GPU rendering at parity with the CPU reference. |
| M5 Production depth | 5 | Path guiding, spectral mode, deep output, splats, plug-ins, bidirectional mode, exact surfaces and the remaining advanced capabilities. |
| M6 Host applications | 5 | Running the renderer inside other content-creation applications and importing content from other renderers. |

## M1 Reference renderer (phase 4)

**Goal.** Render scenes described in OpenUSD, MaterialX and Arc Scene packages on the CPU, with
deterministic output, OpenEXR outputs and a scriptable CLI.

**Scope.**

- Phase-4 T1 rows of SCENE (RB-SCENE-001, 002, 004 to 012, 014, 016 to 021, 023 to 025), CAM
  (RB-CAM-001 to 003, 010, 015, 016, 018), GEOM (RB-GEOM-001 to 003, 006, 007, 009, 011, 012, 014,
  019 to 022, 026 to 028), VOL (RB-VOL-001, 004 to 006, 008, 010), LIGHT (RB-LIGHT-001 to 005,
  007 to 010, 013 to 015, 017, 019 to 021, 028), SURF (RB-SURF-001 to 007, 009 to 012, 014, 015,
  017, 019), NODE (RB-NODE-001 to 013, 015, 016, 019, 021), TEX (RB-TEX-001, 002, 004, 005,
  007 to 010, 013 to 015), PATH (RB-PATH-001 to 003, 005 to 007, 013 to 018), OUT (RB-OUT-001 to
  004, 006 to 009, 012 to 016), COLOR (RB-COLOR-001 to 004, 006, 007), DIAG (RB-DIAG-001 to 003,
  009 to 012), IO (RB-IO-001, 003, 008 to 015).
- RB-NOISE-001 to 003, RB-NOISE-008, RB-NOISE-012.
- RB-BATCH-001 to 008, RB-BATCH-011.
- RB-EXT-001 to 003, RB-EXT-005.

**Dependencies.** `arc-core` L0 to L4: `arc-base`, `arc-mesh`, `arc-doc`, `arc-command`,
`arc-scene` (contract types and material model), `arc-nodes`, `arc-io-usd`, `arc-io-mesh`. No other
Arc product.

**Exit criteria.**

- The CLI renders every scene of the M1 corpus from OpenUSD with MaterialX looks and writes
  multipart OpenEXR with beauty, lighting components, data outputs and Cryptomatte.
- Rendering the corpus with 1, 7 and all threads, and interrupted and resumed, yields identical
  image hashes.
- Furnace tests pass for every OpenPBR lobe combination, and OpenPBR reference scenes pass the
  conformance tolerance.
- A render package created on one machine renders bit-identically on another machine of the same
  platform.
- Every corpus importer has been fuzzed in continuous integration without a crash.

## M2 Look development and interactive rendering (phase 4)

**Goal.** Make RenderBuilder a desktop look-development tool and complete the professional feature
set of the engine.

**Scope.**

- Phase-4 rows of VIEW (RB-VIEW-001 to 018), LOOKDEV (RB-LOOKDEV-001 to 013), OVR (RB-OVR-001 to
  013), IMGR (RB-IMGR-001 to 012) and LIB (RB-LIB-001 to 011).
- RB-NOISE-004 to 007, RB-NOISE-009, RB-NOISE-010.
- Phase-4 T2 and T3 rows of SCENE (RB-SCENE-003, 013, 015, 022), CAM (RB-CAM-004 to 009, 011, 012,
  017, 019 to 022), GEOM (RB-GEOM-004, 008, 010, 013, 017, 024), VOL (RB-VOL-003, 007, 009, 013),
  LIGHT (RB-LIGHT-006, 011, 012, 016, 018, 022 to 027), SURF (RB-SURF-008, 013, 016, 018, 020), NODE
  (RB-NODE-014, 017, 018), TEX (RB-TEX-003, 006, 011, 016), PATH (RB-PATH-004, 008, 009), OUT
  (RB-OUT-005), COLOR (RB-COLOR-005, 008, 009), DIAG (RB-DIAG-004 to 008), IO (RB-IO-002, 004 to
  006).
- Phase-4 rows of STYL (RB-STYL-001 to 004, 006 to 009).

**Dependencies.** M1; `arc-core` L5 and L6: `arc-gpu`, `arc-ui` (node editor, docking, property
grid), `arc-app`. Arc Hub Documents module for Hub libraries (RB-LIB-010).

**Exit criteria.**

- Workflow W1 (look development with a turntable) passes its acceptance in the desktop application.
- Workflow W7 (compositing hand-off) passes its acceptance.
- Workflow W5 steps 2 to 5 pass on a character delivered as an OpenUSD stage.
- A material parameter edit produces updated pixels within the latency target on the interactive
  corpus.
- Every render view function is operable by keyboard alone and announced to a screen reader.

## M3 Hosts, studies and farm (phase 4)

**Goal.** Render from inside Arc products through the Arc Scene contract, answer daylight and
illuminance questions, and run animation on a self-hosted farm.

**Scope.**

- Phase-4 rows of HOST (RB-HOST-001 to 016) and STUDY (RB-STUDY-001 to 010, 012 to 017).
- RB-BATCH-010, RB-BATCH-012, RB-BATCH-014 to 021.

**Dependencies.** M2; `arc-scene` session protocol and `arc-hub-api` in `arc-core`; the Arc Hub
render service and Documents module; ArchBuilder (architectural core, phase 2) and SolidBuilder
(phase 3) as first hosts, MeshBuilder (phase 4) when its scene export is available.

**Exit criteria.**

- Workflows W2, W3, W4, W8 and W9 pass their acceptance.
- Terminating the session process during a host render never terminates or blocks the host, and a
  restarted session reaches the host's current revision.
- Illuminance validation cases pass within the documented tolerance.
- A tiled hero frame merged from 16 workers is bit-identical to a single-process render.

## M4 GPU device (phase 5)

**Goal.** GPU rendering on any vendor's hardware, at parity with the CPU reference.

**Scope.** RB-PATH-019 to 023, RB-BATCH-009, RB-TEX-012, RB-NOISE-013.

**Dependencies.** M1 to M3 corpus and parity tests; wgpu ray queries on target platforms; the
suite's phase 5 GPU path tracing theme.

**Exit criteria.**

- Workflow W10 passes on GPUs from at least two vendors.
- Every corpus scene the GPU feature matrix supports passes the parity test; every unsupported
  feature is reported by validation.
- Interactive camera navigation in the render view uses the GPU device with reduced-resolution
  reconstruction on every supported vendor.

## M5 Production depth (phase 5)

**Goal.** Complete the advanced and differentiating capabilities.

**Scope.** RB-PATH-010 to 012, RB-PATH-024, RB-PATH-025, RB-COLOR-010, RB-CAM-013, RB-CAM-014,
RB-GEOM-005, RB-GEOM-015, RB-GEOM-016, RB-GEOM-018, RB-GEOM-023, RB-GEOM-025, RB-VOL-002,
RB-VOL-011, RB-VOL-012, RB-SURF-021, RB-STYL-005, RB-STYL-010, RB-STYL-011, RB-NODE-020,
RB-NODE-022, RB-TEX-017, RB-NOISE-011, RB-OUT-010, RB-OUT-011, RB-IMGR-013, RB-IMGR-014,
RB-BATCH-013, RB-HOST-017, RB-STUDY-011, RB-LIB-012, RB-EXT-004, RB-EXT-006 to 012, RB-IO-007,
RB-PATH-026, RB-PATH-027, RB-GEOM-029 to 031.

**Dependencies.** M4 for GPU coverage of new features; `arc-nodes` job graphs; the suite plug-in
host for WebAssembly plug-ins.

**Exit criteria.**

- On the interior corpus, path guiding reaches the adaptive threshold in less time than unguided
  rendering at equal settings, with identical results across thread counts.
- Spectral renders of the dispersion and thin-film scenes match the analytical references within
  tolerance.
- Workflow W6 passes with deep output, and a compositing tool merges the deep images correctly.
- A procedural plug-in that exceeds its memory limit fails its task without terminating the
  render process.
- On the caustic corpus, the bidirectional mode converges to the unidirectional reference within
  tolerance and reaches the adaptive threshold in less time at equal settings; the interior corpus
  stays on the unidirectional default.
- NURBS and limit-surface scenes render with a reported maximum deviation below the sub-pixel
  tolerance, without cracks at patch and trim boundaries, and identically across thread counts.

## M6 Host applications (phase 5)

**Goal.** Run the renderer inside other content-creation applications and bring in content from
other renderers, without loading the engine into any host.

**Scope.** RB-HOSTAPP-001 to 017, RB-IO-016 to 018.

**Dependencies.** M3 (session protocol, frame ring, render packages); the connector repository
created with the first connector (suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md));
OpenUSD with Hydra for the delegate; M5 for host-selected integrator modes and exact surfaces.

**Exit criteria.**

- The delegate renders a viewport in a Hydra-capable host and in the OpenUSD command-line viewer,
  and terminating the session process leaves the host running (ADR 0007).
- A connector renders a scene from inside its host with the render view in the host, updates
  interactively while the user edits, and produces the same image from a batch export on the farm.
- Every translation of the connector corpus produces a loss report, and no material is approximated
  without an entry.
- Content from a format with a published specification imports with a loss report on the
  interchange corpus.
