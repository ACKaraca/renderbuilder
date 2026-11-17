# Architecture decision records

Decisions specific to RenderBuilder. Suite-wide decisions (repository topology, UI toolkit,
document model, command model, interoperability policy, rendering through the Arc Scene contract,
licensing and others) are in the
[suite ADRs](https://github.com/ACKaraca/arc-suite/tree/main/docs/adr) and are not restated here.

## Index

| ADR | Title | Status |
|---|---|---|
| [0001](0001-cpu-reference-integrator-first.md) | CPU reference integrator first, GPU integrator on wgpu held to parity | Accepted |
| [0002](0002-shading-program-ir.md) | One shading program representation for all devices | Accepted |
| [0003](0003-owned-acceleration-structures.md) | Acceleration structures owned in Rust | Accepted |
| [0004](0004-renderer-as-session-process.md) | Every entry point drives the renderer through a session | Accepted |
| [0005](0005-native-libraries-behind-ffi-crates.md) | Native libraries behind isolated FFI crates | Accepted |
| [0006](0006-render-document-layers-over-sources.md) | The render document layers over scene sources | Accepted |

## Template

```markdown
# NNNN. Title

- Status: Proposed | Accepted | Superseded by NNNN
- Capabilities: RB-...

## Context

What forces the decision.

## Decision

What is decided, stated so it can be checked.

## Consequences

What becomes easier, harder or required.

## Alternatives considered

Each option and why it was not chosen.

## Reopen when

The observable trigger that would justify revisiting the decision.
```
