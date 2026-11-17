# Statistics, logging and profiling

This area covers what RenderBuilder reports about a render: logs, statistics, reports, profiles,
debug shading and memory behavior.

Domain objects: log record, statistics record, render report, profile trace, debug mode.

## Logs and statistics

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-DIAG-001 | Logging | Log records carry level (error, warning, info, debug), subsystem and prim path, go to console, file or JSON, and repeated warnings collapse into one record with a count. | T1 | 4 |
| RB-DIAG-002 | Render statistics | Time per phase (load, acceleration build, texture loading, shading, sampling, imagers), peak memory per category, rays by type, texture cache hit rates and sample counts are written as JSON per frame. | T1 | 4 |
| RB-DIAG-003 | Scene statistics | Counts of prims, instances, unique and instanced triangles, curves, points, lights and textures, with memory per prim. | T1 | 4 |
| RB-DIAG-004 | Render report | A self-contained HTML and JSON report per render combines statistics, settings, warnings and dependencies. | T2 | 4 |
| RB-DIAG-005 | Texture usage report | Per texture: memory used, resolution read versus available, and lookups, revealing oversized textures. | T2 | 4 |

## Profiling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-DIAG-006 | Shading cost profile | Time is attributed to materials and nodes, and a per-pixel cost output shows where render time goes. | T2 | 4 |
| RB-DIAG-007 | Trace files | Per-thread timelines export in the Chrome trace event format. | T2 | 4 |
| RB-DIAG-008 | Determinism check | A CLI command renders twice with different thread counts and compares image hashes. | T3 | 4 |

## Debug views

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-DIAG-009 | Debug shading modes | One-step modes show normals, UVs, facing ratio, wireframe, object and primitive identifier colors, barycentrics, occlusion, albedo only, roughness only and emission only. | T1 | 4 |
| RB-DIAG-010 | Ignore toggles | Motion blur, depth of field, subdivision, displacement, bump, textures, subsurface, volumes, atmosphere, imagers and lights by type can be ignored for diagnosis without editing the scene. | T1 | 4 |

## Failure handling

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-DIAG-011 | Memory exhaustion | When a memory budget is exceeded, the render stops with a report of the largest consumers instead of crashing the process. | T1 | 4 |
| RB-DIAG-012 | Crash reports | A fault produces a report with log, scene digest and settings, submitted only after the user agrees. | T1 | 4 |
