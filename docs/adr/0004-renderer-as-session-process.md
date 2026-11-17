# 0004. Every entry point drives the renderer through a session

- Status: Accepted
- Capabilities: RB-HOST-001, RB-HOST-005, RB-BATCH-011, RB-BATCH-015, RB-VIEW-001

## Context

Suite ADR-0010 places photoreal rendering in a separate process for host products. RenderBuilder
itself has four entry points: its desktop application, host viewports, the CLI and farm workers. If
each used the engine differently, behavior would diverge (an image configured in a host would render
differently on the farm) and four integrations would need testing.

## Decision

- `renderbuilder-session` is the only interface to the engine. Hosts, the desktop application, the
  CLI and the farm worker all open sessions, send a snapshot (from a live model, a document or a
  package), apply deltas and issue render requests.
- Hosts always run the session in a separate process. The RenderBuilder desktop application runs
  its own session in a child process by default, so an engine fault does not lose unsaved document
  edits; an in-process mode exists for debugging.
- The farm worker is a mode of `renderbuilder-cli` (`worker`), not a separate application.

## Consequences

- One code path to test, profile and harden; the session protocol decoder is fuzzed once.
- Interprocess overhead applies to the desktop application as well; shared-memory frame transfer
  keeps it small.
- Session protocol compatibility becomes a public contract governed by the suite's versioning rules.

## Alternatives considered

- **Engine linked directly into the desktop application and CLI, IPC for hosts only.** Less
  overhead, but two integration paths and no crash isolation for document edits.
- **A separate farm-worker application.** Clearer naming, but duplicates CLI packaging and
  configuration.

## Reopen when

Profiling shows the interprocess path costs more than 10 percent of interactive update latency in
the desktop application after shared-memory transfer is in place.
