# 0007. The Hydra delegate and host connectors contain no engine

- Status: Accepted
- Capabilities: RB-HOSTAPP-001, RB-HOSTAPP-002, RB-HOSTAPP-006, RB-HOSTAPP-017, RB-HOST-005

## Context

RenderBuilder runs inside other content-creation applications in two ways: a render delegate for
hosts that implement the Hydra rendering interface, and connectors that read a host's scene through
the host's public API (suite
[ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md)).
Both are native code loaded into the host process, in the language and with the toolchain the host
requires. A delegate that linked the renderer would put a large, memory-hungry engine into the host's
address space: a renderer fault would end the host session and lose the user's unsaved work, and the
engine's build would have to track every host's compiler and runtime.

## Decision

- The delegate and every connector are thin session clients. They translate the host's scene,
  settings and change notifications into Arc Scene snapshots and deltas, and display frames read from
  the shared-memory ring. They contain no integrator, shading, acceleration or texture code.
- The renderer always runs as a `renderbuilder-session` process started from the installed
  RenderBuilder, never inside the host (ADR 0004). This answers the earlier question of whether a
  delegate should exist: it does, in this form.
- Delegate and connectors live in the connector repository named by the suite decision, are versioned
  against the Arc Scene contract, and are outside the Rust-only rule because they are not Arc desktop
  applications. This repository specifies their behavior and owns the session side.
- A connector never replaces a core workflow: everything it carries can also be exchanged through
  OpenUSD, MaterialX or an Arc Scene package.

## Consequences

- A renderer fault, an out-of-memory condition or a hung render never terminates the host.
- Frame transfer and scene translation add interprocess cost, bounded by shared memory and
  coalesced deltas.
- The Arc Scene contract is the compatibility surface; host version changes affect only the thin
  client.
- Translation fidelity is limited by what each host's public API exposes; gaps are reported through
  loss reports (RB-HOSTAPP-016).

## Alternatives considered

- **Engine linked into the delegate.** Lower latency, but no crash isolation and a build tied to
  every host's toolchain.
- **Export-only integration (render packages).** Simple, but no render view in the host and no
  interactive updates.

## Reopen when

Measured interprocess overhead exceeds 10 percent of interactive update latency in a host after
shared-memory transfer is in place, or a major host stops exposing the public API a connector needs.
