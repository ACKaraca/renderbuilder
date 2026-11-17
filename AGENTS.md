# AGENTS.md

## What this repository is

RenderBuilder: the physically based production renderer and look-development tool of the Arc suite.
It renders OpenUSD, MaterialX, OpenPBR and Arc Scene content with a deterministic path tracer and
serves the desktop application, Arc product viewports, the CLI and render-farm workers.

## Status and where to start

Design phase: no code and no builds exist yet. Read in this order: `README.md`,
`docs/product/vision.md`, `docs/product/capabilities/README.md`, `docs/architecture/overview.md`,
`docs/architecture/integrator.md`, `docs/architecture/arc-scene-integration.md`, `docs/roadmap.md`.

## Repository layout (target)

```
crates/
  renderbuilder-model/      render document: setups, looks, overrides, studies (headless)
  renderbuilder-commands/   commands on the model (headless)
  renderbuilder-io/         Arc Scene, OpenUSD, MaterialX, VDB and other mappings (headless)
  renderbuilder-shade/      shading network compilation to the shading program (headless)
  renderbuilder-engine/     render scene, acceleration, CPU integrator, textures, film (headless)
  renderbuilder-gpu/        wgpu device (headless, phase 5)
  renderbuilder-session/    Arc Scene session server, IPC, frame streaming, job runner
  renderbuilder-ui/         render view and editors (egui)
  renderbuilder-oidn-sys/   FFI: Open Image Denoise (unsafe allowed)
  renderbuilder-ocio-sys/   FFI: OpenColorIO (unsafe allowed)
apps/
  renderbuilder/            desktop application (wiring only)
  renderbuilder-cli/        render, validate, package, texture, denoise, merge, parity, serve, worker
docs/
xtask/
```

## Commands

No build exists yet. Once code lands, these apply:

- `cargo fmt --check`
- `cargo clippy --workspace --all-targets -- -D warnings`
- `cargo nextest run --workspace`
- `cargo xtask layers`

## Suite rules

These rules apply in every Arc repository. The canonical text is
[`docs/engineering/agent-rules.md`](https://github.com/ACKaraca/arc-suite/blob/main/docs/engineering/agent-rules.md)
in `arc-suite`; change it there first.

1. **A person is accountable.** Whoever runs an agent answers for what it submits.
2. **Start from an issue.** Search issues and pull requests, check that nobody else is assigned,
   assign yourself and set the status to *In Progress* before changing files. When you finish,
   comment with what changed, the commit or pull request link and open items, then update the status.
   *Done* requires met and verified acceptance criteria.
3. **Verify, do not guess.** Open a definition before using it. Confirm a cause in the code.
4. **The document is the authority and everything is a command.** No change reaches a document
   except through a validated command inside a transaction. Programmatic paths never open dialogs.
5. **Respect the layers.** A crate depends only on lower layers. Nothing below the UI layer depends
   on windowing, GPU or UI crates. Products never depend on other products.
6. **Never crash, never lose work.** No `unwrap`, `expect`, `panic!`, `todo!` or `unreachable!` in
   production code. No `unsafe` outside the FFI crates named in an ADR. Treat every file and message
   as hostile: bound recursion, allocation and counts. A crash fix ships with a regression test.
7. **Clean room.** Implement from published specifications, observed behavior and original
   reasoning. Never copy or consult another product's code, decompiled output, file-format reverse
   engineering, documentation text, icons, fonts, templates or sample files. Do not name our
   features after another company's features.
8. **Licenses.** Add a dependency only if its license is on the allowlist in
   `docs/engineering/dependency-policy.md` in `arc-suite`. Record every asset's origin and license.
9. **Rust only in desktop applications.** No web view, no JavaScript or TypeScript.
10. **Small, tested changes.** One concern per pull request, with the test that fails without it.
    Update the documents that describe the behavior you change in the same pull request.
11. **Report only what ran.** Say what you tested and what you could not run.
12. **Ask a human before** adding a license outside the allowlist, publishing or releasing,
    changing a file format or public protocol, adding a new repository or remote, spending money,
    handling secrets, or any destructive git operation. Otherwise keep working.
13. **Keep secrets and personal data out** of repositories, logs and requests.

## Repository invariants

- Determinism: the CPU integrator gives bit-identical images for identical inputs on a platform,
  regardless of threads, tiles or resume. Never draw randomness from shared generators; use the
  pixel's sample sequence at a fixed dimension (`docs/architecture/integrator.md`).
- The CPU integrator is the reference. A GPU feature ships only with passing parity tests and a
  feature-matrix entry; unsupported features fail validation, never render differently.
- One shading program for all devices: materials compile from MaterialX or OSL into the IR in
  `renderbuilder-shade`. No WebAssembly in shading.
- Engine, shade, io, model and session crates build headless: no windowing, UI or GPU dependency
  (only `renderbuilder-gpu` uses wgpu, for compute).
- Every entry point (desktop, hosts, CLI, worker) goes through `renderbuilder-session`. Hosts never
  link the engine.
- Looks and overrides never write into scene sources; prims are addressed by stable identity.
- Frames carry the revision they were rendered from; a stale frame is never shown or written as
  current.
- `unsafe` only in `renderbuilder-oidn-sys` and `renderbuilder-ocio-sys` (ADR 0005).
- Sessions listen only on local endpoints with a session token; validate every message size and
  count before allocating.
- Output files are written to a temporary name and renamed atomically.

## Documentation rules

- No trace of planning sources; never describe a feature as "like" another product or name it after
  another company's feature. Other companies' names appear only as formats and standards, in factual
  interoperability statements, once in the vision's positioning paragraph, and for libraries in
  architecture documents.
- Use descriptive, generic vocabulary. Write behavior in original words.
- Each fact once: link suite-wide rules in `arc-suite` instead of copying them.
- Capability identifiers `RB-<AREA>-<NNN>` are permanent; status lives in issues, not in tables.
- American English, present tense, no marketing language, no claims of implementation.

## Issue workflow

Search issues and pull requests; confirm nobody is assigned; assign yourself and set *In Progress*.
Statuses: Backlog, Ready, In Progress, In Review, Blocked, Done (only with met, verified acceptance
criteria). On finishing, comment with what changed, the commit or pull request link and open items,
update the status and file follow-up issues after checking for duplicates. Labels: `type: …`,
`tier: 1|2|3`, `phase: 0…5`, `area: <token>`, `status: blocked`, `needs: verification`. Commits use
Conventional Commits with a DCO sign-off.

## Verification

Run the narrowest relevant check first (the crate's tests, the affected corpus scenes, the parity
test for GPU changes). Report exactly what ran and what could not be run.

## Ask a human about

- Changes to the session protocol, render package, library package or `.renderb` schema.
- Adding a native library or any new `unsafe` crate.
- Relaxing determinism or parity thresholds.
- Anything listed in suite rule 12.
