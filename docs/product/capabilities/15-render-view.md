# Interactive render view

This area covers the render view panel: interactive progressive rendering, image inspection,
snapshots and comparison, and accessibility of the view.

Domain objects: render view, interactive session, snapshot, comparison.

## Interactive rendering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VIEW-001 | Interactive rendering | The render view refines progressively and restarts within the latency targets after any edit to scene, materials, lights, camera or settings; only data affected by the edit is rebuilt. | T1 | 4 |
| RB-VIEW-002 | Pause and resume | Pausing keeps accumulated samples and releases compute threads; resuming continues from the same samples. | T1 | 4 |
| RB-VIEW-003 | Region refinement | A drawn region concentrates sampling inside it while the rest of the image stays as rendered. | T1 | 4 |
| RB-VIEW-004 | Camera navigation | Orbit, pan, dolly and fly navigation in the render view moves the setup camera or a temporary view camera, with reduced-resolution passes while moving. | T1 | 4 |
| RB-VIEW-005 | Isolate selection | Rendering can be limited to the selected prims and lights temporarily without changing the document. | T2 | 4 |
| RB-VIEW-006 | Update lock | Interactive updates can be suspended while several edits are made and applied together. | T2 | 4 |
| RB-VIEW-007 | Multiple render views | Several render views show different setups or cameras of the same document at once, sharing scene data. | T3 | 4 |

## Image inspection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VIEW-008 | Output display | Any output, layer or channel can be displayed, with false color for identifiers and a normalized range for depth and position. | T1 | 4 |
| RB-VIEW-009 | Pixel inspector | Shows the pixel's values in rendering and display space, sample count, prim path and material under the cursor. | T1 | 4 |
| RB-VIEW-010 | Pick to select | Clicking a pixel selects the prim or material in the document and in a connected host session. | T1 | 4 |
| RB-VIEW-011 | Display adjustments | Display exposure and gamma sliders change only what is shown, never the outputs. | T1 | 4 |
| RB-VIEW-012 | Zoom and pan | Zoom to fit, one-to-one and arbitrary zoom with pan, correct on high-density displays. | T1 | 4 |
| RB-VIEW-013 | Progress and status | A status bar shows pass, samples per pixel, elapsed and estimated remaining time and memory use. | T1 | 4 |

## Snapshots and comparison

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VIEW-014 | Snapshots | The current image is stored as a snapshot with a settings digest, timestamp and note, listed in a history strip saved with the document. | T1 | 4 |
| RB-VIEW-015 | A/B comparison | Two images (snapshots or a snapshot and the live render) compare by split wipe, toggle or difference. | T1 | 4 |
| RB-VIEW-016 | Snapshot restore | The settings and material state recorded in a snapshot can be re-applied as one undoable command. | T2 | 4 |
| RB-VIEW-017 | Save from view | The displayed image or all outputs of the current render save to files with the setup's file templates or a chosen path. | T1 | 4 |

## Accessibility

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VIEW-018 | Keyboard and screen reader | Every render view function is reachable by keyboard; progress milestones and completion are announced to screen readers. | T1 | 4 |
