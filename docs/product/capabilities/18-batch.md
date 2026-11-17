# Command-line renderer, batch and render-farm worker

This area covers headless rendering: the command-line renderer, distributed rendering of single
frames and sequences, and the render-farm worker that serves the Arc Hub render service and any
other queue manager.

Domain objects: render invocation, job description, render job, frame packet, tile, worker,
estimate.

## Command-line renderer

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-BATCH-001 | Render command | Renders a document's setup over a frame range (start, end, step or list) and camera list, writing the setup's outputs or overridden output paths. | T1 | 4 |
| RB-BATCH-002 | Accepted inputs | `.renderb` documents, render packages, OpenUSD stages with a chosen render settings prim and Arc Scene packages render without opening a desktop application. | T1 | 4 |
| RB-BATCH-003 | Invocation overrides | Any setting or prim attribute can be overridden by path on the command line or from a JSON file, without modifying the input. | T1 | 4 |
| RB-BATCH-004 | Progress and exit codes | Progress, warnings and results stream as JSON lines; exit codes distinguish success, render errors, invalid input, missing assets, resource exhaustion and cancellation. | T1 | 4 |
| RB-BATCH-005 | Validate command | Runs scene validation (RB-SCENE-024) without rendering and returns a machine-readable report. | T1 | 4 |
| RB-BATCH-006 | Package command | Writes a render package (RB-SCENE-005) with assets resolved, copied or referenced by digest, and paths made relative. | T1 | 4 |
| RB-BATCH-007 | Dependency command | Prints the dependency list of a document or package (RB-SCENE-019). | T1 | 4 |
| RB-BATCH-008 | Resource limits | Thread count, reserved cores, process priority and a soft memory budget, to which the texture cache and stand-in loading adapt. | T1 | 4 |
| RB-BATCH-009 | GPU selection | The device and GPU adapters are chosen by name, index or minimum memory. | T2 | 5 |
| RB-BATCH-010 | Sequence resume | Existing complete frames are skipped, so an interrupted sequence restarts where it stopped. | T2 | 4 |
| RB-BATCH-011 | Serve mode | The CLI serves the Arc Automation Protocol over stdio or a local socket, exposing every render command for scripts. | T1 | 4 |

## Distributed rendering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-BATCH-012 | Tile split and merge | One frame renders as tiles in separate processes or machines and a merge step assembles all outputs, including Cryptomatte and deep data, identical to a single-process render. | T2 | 4 |
| RB-BATCH-013 | Sample split and merge | Several machines render disjoint sample ranges of one frame and a merge combines them into the same image a single machine would produce. | T3 | 5 |
| RB-BATCH-014 | Frame packets | Sequences divide into packets of consecutive frames so a worker reuses loaded scene data across frames. | T1 | 4 |

## Render-farm worker

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-BATCH-015 | Hub render worker | A long-running worker takes jobs from the Arc Hub render service through the Hub API, fetches packages into a content-addressed cache, renders, uploads outputs and reports progress and logs. It needs only a Hub service identity. | T1 | 4 |
| RB-BATCH-016 | Queue-manager compatibility | Any queue manager can drive rendering through the plain CLI, a job description file and the documented exit codes. | T1 | 4 |
| RB-BATCH-017 | Package integrity | Asset digests are verified before rendering; a mismatch fails the job before any frame starts. | T1 | 4 |
| RB-BATCH-018 | Worker policy | A worker sets concurrent jobs, memory limit, GPU assignment and an idle-only schedule for workstations that join the farm after hours. | T2 | 4 |
| RB-BATCH-019 | Failure categories | Failed frames report a category (out of memory, missing asset, invalid scene, renderer fault, time limit) so the queue can retry with adjusted resources. | T2 | 4 |
| RB-BATCH-020 | Job estimate | A preflight renders sample frames at reduced cost and estimates time and memory per frame for queue planning. | T2 | 4 |
| RB-BATCH-021 | Result publishing | Outputs publish to Arc Hub as versions with thumbnails and setup metadata; notifications are left to Hub. | T2 | 4 |
