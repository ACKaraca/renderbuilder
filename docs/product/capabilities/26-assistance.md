# Assistance

This area specifies what the optional assistant does in RenderBuilder: explaining render time and
noise, proposing render settings for a time budget, setting up materials, lighting, cameras and
looks from a description or reference, preparing studies, and disclosing learned image filters,
always as previewed commands whose effect is measured by short renders and the renderer's own
statistics. The suite rules for levels, grounding, data handling, provenance and evaluation are in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md); the
render commands the assistant uses are [RB-EXT-001](23-extensibility.md). Every capability here also
has a manual and scripted path.

## Domain objects

| Object | Key properties | Relationships |
|---|---|---|
| Assistance request | Text, level, scope (setups, looks, materials, lights, cameras), reference images, base revision | Produces answers or proposals |
| Proposal | Items, command invocations, calibration renders, measured time and noise, checker findings | Bound to a base revision; applied as one undo step |
| Calibration render | Region, resolution, sample budget, measured time per sample, noise estimate | Rendered locally to measure a proposed setting |
| Filter disclosure | Filter name, version, kind (deterministic or learned), outputs affected | Written into output metadata and render reports |

## Panel and questions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-ASSIST-001 | Assistant panel and endpoint | An optional assistant panel and a Model Context Protocol endpoint derived from the command registry give a user-configured provider, including local models, the render commands, queries and statistics with the suite's preview, permission and data rules; no feature requires them. | T3 | 4 |
| RB-ASSIST-002 | Render time and noise explained | The assistant answers "why is this frame slow" or "where does the noise come from" from render and scene statistics, the shading cost profile and the texture usage report (RB-DIAG-002, RB-DIAG-003, RB-DIAG-005, RB-DIAG-006), naming the materials, lights and prims responsible and citing the figures. | T3 | 4 |
| RB-ASSIST-003 | Diagnostics explained | For warnings, render reports and memory exhaustion reports (RB-DIAG-001, RB-DIAG-004, RB-DIAG-011), the assistant groups the messages by cause, explains them and proposes scene or setting changes. | T3 | 4 |

## Settings and look

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-ASSIST-004 | Settings for a time budget | Given a target time per frame, resolution and quality, the assistant proposes sampling, adaptive stop conditions and frame time limits (RB-NOISE-002, RB-NOISE-004), light sampling and texture settings; short calibration renders on the user's machine measure time and noise for the proposal, and the panel shows the measured values, not estimates by the model. | T3 | 4 |
| RB-ASSIST-005 | Materials from a description | From a description such as "aged oak floor, satin finish" the assistant proposes an appearance template (RB-LIB-007) with parameters or an OpenPBR network, previewed in the material preview (RB-LOOKDEV-003); inline diagnostics (RB-LOOKDEV-005) and a plausibility check of albedo, roughness and energy conservation run on the proposal. | T3 | 4 |
| RB-ASSIST-006 | Materials from a reference image | From a reference photograph the assistant proposes material parameters and textures, and renders the proposal under lighting matched to the reference so the user compares them side by side. | T3 | 5 |
| RB-ASSIST-007 | Lighting from a description | From a description such as "late afternoon in October, warm, soft interior light" the assistant proposes sun position from the document's location and a date and time (RB-STUDY-001), sky model, scenario preset (RB-STUDY-003) and exposure; a preview render reports clipped and underexposed pixel fractions before apply. | T3 | 4 |
| RB-ASSIST-008 | Camera and composition | For a chosen object or room the assistant proposes camera positions, focal lengths and framing as a set of thumbnail renders, each with its camera parameters, from which the user picks; chosen cameras are added as cameras of the setup. | T3 | 4 |
| RB-ASSIST-009 | Look variants | The assistant proposes light mixer states (RB-IMGR-006) and imager chains (RB-IMGR-001 to RB-IMGR-005) for a requested mood, previewed on the accumulated samples without restarting the render. | T3 | 4 |

## Studies and disclosure

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-ASSIST-010 | Study setup and results | From a request such as "illuminance on the office desks at 0.75 m, March equinox at noon", the assistant proposes sensor grids, sky, date and physically valid mode (RB-STUDY-005, RB-STUDY-007); after the study runs it explains the study report (RB-STUDY-008) and never states that a design meets a lighting standard. | T3 | 4 |
| RB-ASSIST-011 | Learned filter disclosure | When a learned filter such as the denoiser (RB-NOISE-008) or a plug-in imager for upscaling or stylization (RB-IMGR-014) affects an output, the output metadata and the render report record the filter, its version and that it is learned; studies in physically valid mode report values computed before any learned filter. | T2 | 4 |
| RB-ASSIST-012 | Learned upscaling and frame interpolation | Through a user-configured model or plug-in, the assistant proposes upscaling stills and interpolating animation frames, with the comparison against a native render of a sample region shown before the result is written; outputs carry the disclosure of RB-ASSIST-011. | T3 | 5 |

## Libraries, scripts and review

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-ASSIST-013 | Library tagging and search | The assistant proposes tags and categories for library items (RB-LIB-001) from their names, parameters and thumbnails, applied after confirmation, and finds items from natural-language searches with the reason for each match. | T3 | 4 |
| RB-ASSIST-014 | Script and job drafting | The assistant drafts Python scripts (RB-EXT-002) and render job graphs (RB-EXT-004) for a described task, such as one study per facade option; they run only when the user starts them. | T3 | 4 |
| RB-ASSIST-015 | Assisted content review | Materials, lights, cameras and setups carrying a provenance mark show a distinct marker and filter; the user reviews and clears marks, and render packages list unreviewed content. | T3 | 4 |

## Readiness

Readiness classes are defined in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md#readiness-classes).

| Class | ID | Checked by | Depends on |
|---|---|---|---|
| R1 | RB-ASSIST-001 | Command schemas and permissions | RB-EXT-001, the suite assistant host in `arc-app` |
| R1 | RB-ASSIST-002 | Render and scene statistics | RB-DIAG-002, RB-DIAG-003, RB-DIAG-005, RB-DIAG-006 |
| R1 | RB-ASSIST-003 | Render reports | RB-DIAG-001, RB-DIAG-004, RB-DIAG-011 |
| R1 | RB-ASSIST-004 | Calibration renders | RB-NOISE-002, RB-NOISE-004 |
| R1 | RB-ASSIST-005 | Inline diagnostics, plausibility check | RB-LIB-007, RB-LOOKDEV-003, RB-LOOKDEV-005 |
| R2 | RB-ASSIST-006 | Side-by-side comparison under matched lighting | Image input in the provider, lighting estimation from a photograph |
| R1 | RB-ASSIST-007 | Exposure statistics of a preview render | RB-STUDY-001, RB-STUDY-003 |
| R1 | RB-ASSIST-008 | Thumbnail renders | Camera commands, image input in the provider |
| R1 | RB-ASSIST-009 | Progressive preview | RB-IMGR-001 to RB-IMGR-006 |
| R1 | RB-ASSIST-010 | Study report | RB-STUDY-005, RB-STUDY-007, RB-STUDY-008 |
| R1 | RB-ASSIST-011 | Output metadata check | RB-NOISE-008, RB-IMGR-014 |
| R3 | RB-ASSIST-012 | Comparison with a native render region | RB-IMGR-014, a user-configured model |
| R1 | RB-ASSIST-013 | User confirmation | RB-LIB-001, RB-LIB-006 |
| R1 | RB-ASSIST-014 | Static check, job preview | RB-EXT-002, RB-EXT-004 |
| R1 | RB-ASSIST-015 | Provenance query | Provenance marks in `arc-core` |

## Evaluation

The assistance task suite is built from the [reference workflows](../workflows.md): W1
(RB-ASSIST-005, RB-ASSIST-009), W2 (RB-ASSIST-004, RB-ASSIST-007, RB-ASSIST-008), W3 and W4
(RB-ASSIST-010), W6 and W8 (RB-ASSIST-002, RB-ASSIST-004) and W9 (RB-ASSIST-005, RB-ASSIST-008).
Thresholds are settled under [OQ-24](../open-questions.md).
