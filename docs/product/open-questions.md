# Open questions

Behaviors that need verification and design decisions that are not yet settled. Each entry names
the capabilities it affects and what closes it. A closed question becomes a capability change or an
ADR and keeps its number here with the resolution.

| ID | Question | Capabilities affected | Closed by |
|---|---|---|---|
| OQ-01 | Which features must the first GPU device support before it is offered for final frames, and which may stay CPU-only (OSL, deep output, nearest-point sampling, some imagers)? | RB-PATH-019, RB-PATH-020, RB-OUT-010, RB-NODE-020, RB-EXT-006 | A feature matrix agreed with users of the beta, plus parity reports on the scene corpus. |
| OQ-02 | Which OpenPBR specification version is the first target, and how are documents authored against a later version handled? | RB-SURF-001, RB-IO-008, RB-IO-009 | A decision tied to the MaterialX release shipped in the first milestone; a migration rule for parameter changes. |
| OQ-03 | Which hair scattering formulation and which dense-hair approximation are used, and how are they validated? | RB-STYL-001, RB-STYL-003 | Comparison renders against a high-depth reference on curly and straight hair test assets. |
| OQ-04 | What is the complete first set of imagers and their parameter ranges? | RB-IMGR-001 to RB-IMGR-012 | User review of the look-development milestone. |
| OQ-05 | What inputs and controls does the temporal sequence denoiser need, and what flicker metric accepts it? | RB-NOISE-011 | A metric on the animation corpus and a decision between Open Image Denoise based temporal filtering and a dedicated filter. |
| OQ-06 | Which built-in color configuration is the default for architectural users, whose deliverables are mostly display-referred stills, versus film users? | RB-COLOR-001, RB-COLOR-002, RB-COLOR-004 | A decision per render setup preset, tested on both user groups. |
| OQ-07 | Which render view features are needed in host viewports and which only in the RenderBuilder application? | RB-VIEW-001 to RB-VIEW-018, RB-HOST-008 | Prototype review with ArchBuilder and MeshBuilder users. |
| OQ-08 | Should RenderBuilder provide a Hydra render delegate so OpenUSD tools outside Arc can drive it, given that a delegate is a native plug-in loaded into another process? | RB-HOST-001, RB-IO-001 | A decision weighing crash isolation against reach; an out-of-process delegate prototype. |
| OQ-09 | How do lighting scenario presets map to the lighting setups architects already use in building-design tools, and which defaults (exposure, sky) does each preset set? | RB-STUDY-003 | Review with architectural visualization users. |
| OQ-10 | How faithfully can appearance assets from building-design tools be mapped to the appearance templates when they arrive through OpenUSD, glTF or MaterialX? | RB-LIB-007, RB-IO-010 | A mapping table tested on exported sample projects created by the project. |
| OQ-11 | Which OSL subset is supported, and is translation of compiled shaders into the shading program sufficient for production shaders? | RB-NODE-022, RB-EXT-006 | A survey of the open-source shader corpus and a translation prototype. |
| OQ-12 | Which path guiding method is used (online mixture models, spatial-directional trees or another), and what are its memory bounds? | RB-PATH-010, RB-PATH-011, RB-PATH-012 | Prototype comparison on the interior and caustic scenes of the corpus at equal time. |
| OQ-13 | On adapters without hardware ray queries, is compute traversal fast enough to justify GPU rendering, or is a minimum hardware level required? | RB-PATH-019 | Measurements on the GPU corpus across vendors and generations. |
| OQ-14 | Is bitwise determinism across operating systems and CPU architectures achievable, or is cross-platform equality statistical only? | RB-PATH-015, RB-BATCH-012, RB-BATCH-013 | Experiments with fixed floating-point settings and portable math functions; a decision recorded in an ADR. |
| OQ-15 | Can deep OpenEXR writing and VDB reading be provided by pure-Rust libraries at production quality, or do they require isolated native libraries? | RB-OUT-010, RB-IO-011 | Conformance tests of candidate crates against the reference files of those projects. |
| OQ-16 | How are Gaussian splats ray traced efficiently together with other geometry, and which relighting model is credible? | RB-GEOM-015, RB-GEOM-016 | Prototype on captured scenes with inserted objects. |
| OQ-17 | What tolerance does the illuminance validation require, and which published test cases form the suite? | RB-STUDY-010, RB-STUDY-005, RB-STUDY-006 | Selection of analytical and published lighting test cases and a tolerance agreed with lighting designers. |
| OQ-18 | Should the render document itself be an OpenUSD layer instead of an Arc document package? | RB-SCENE-001, RB-SCENE-005 | Revisit after the first milestone if look and override data map cleanly to OpenUSD (see ADR 0006). |
| OQ-19 | Should a user-provided image model (for stylization or upscaling) run through a plug-in imager, and how is frame-to-frame consistency reported? | RB-IMGR-014, RB-EXT-009 | Decision after the plug-in system exists; a model-runtime survey within the dependency policy. |
| OQ-20 | Which glare metric and which viewpoints are meaningful for design decisions? | RB-STUDY-011 | Review with daylight designers. |
| OQ-21 | How does Alembic reading fit the dependency policy (native library behind an FFI crate, or a pure-Rust reader)? | RB-IO-007, RB-GEOM-022 | A license and quality review of available readers. |
