# 0005. Native libraries behind isolated FFI crates

- Status: Accepted
- Capabilities: RB-NOISE-008, RB-COLOR-001, RB-COLOR-008, RB-IO-011, RB-OUT-012

## Context

Some reference implementations of open standards exist only as C or C++ libraries: Open Image
Denoise and OpenColorIO in particular. Reimplementing a trained denoiser or the full color
transform language would be large and error-prone. Suite ADR-0015 forbids `unsafe` outside FFI
crates named in an ADR, and the suite dependency policy restricts licenses.

## Decision

- Open Image Denoise (Apache-2.0) is used through `renderbuilder-oidn-sys`; OpenColorIO
  (BSD-3-Clause) through `renderbuilder-ocio-sys`. These are the only RenderBuilder crates where
  `unsafe` is allowed. Each wraps the library in a small safe API owned by the engine.
- OpenEXR, OpenVDB/NanoVDB reading, OpenUSD and MaterialX are handled by Rust code (in `arc-core`
  format crates or `renderbuilder-io`) unless OQ-15 or OQ-21 conclude otherwise; any additional
  native library requires amending this ADR.
- The engine keeps working without the denoiser library (denoising disabled with a warning). The
  color library is required for custom configurations; the built-in configuration's transforms are
  also implemented natively and cross-checked against the library in tests.

## Consequences

- Production-grade denoising and full color-configuration compatibility without reimplementation.
- Build and packaging must handle two native libraries on all platforms.
- Fuzzing and sanitizers cover the FFI boundaries.

## Alternatives considered

- **Pure Rust only.** No native code, but no comparable denoiser exists and color-configuration
  compatibility would be partial.
- **Native libraries in a separate process.** Stronger isolation, but frame-rate denoising across a
  process boundary costs bandwidth and latency; both libraries are permissively licensed, so the
  byte-level bridge of suite ADR-0009 is not required.

## Reopen when

A Rust implementation passes the library's own conformance tests for either component, or a
library's license changes outside the allowlist.
