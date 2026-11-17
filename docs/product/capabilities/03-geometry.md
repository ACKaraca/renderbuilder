# Geometry

This area covers every renderable surface representation: polygon meshes, subdivision and
displacement, NURBS surfaces and limit surfaces, curves, points and Gaussian splats, analytic
primitives, instancing, and stand-ins that load content on demand.

Domain objects: mesh, subdivision mesh, NURBS surface, curve set, point set, splat set, instancer, prototype,
stand-in, procedural.

## Polygon meshes

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-001 | Polygon meshes | Meshes with faces of any vertex count are triangulated internally with consistent winding and keep per-face material indices. Non-manifold topology renders without error. | T1 | 4 |
| RB-GEOM-002 | Normals and tangents | Authored per-vertex or per-face-vertex normals are used when present; otherwise normals are generated with an angle threshold. Tangent frames for normal maps are generated with the MikkTSpace convention so baked maps match. | T1 | 4 |
| RB-GEOM-003 | UV sets | Meshes carry any number of named UV sets, including UDIM ranges; one set is the default for nodes that do not name one. | T1 | 4 |
| RB-GEOM-004 | Shading normal correction | Interpolated shading normals that face away from the viewer are bent toward the geometric normal, removing black faceting artifacts on coarse meshes. | T2 | 4 |
| RB-GEOM-005 | Compact geometry storage | Positions, normals and indices are stored at reduced precision where the error stays within the scene tolerance, reducing memory for very large meshes; the setting is reported in statistics. | T3 | 5 |

## Subdivision and displacement

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-006 | Subdivision surfaces | Catmull-Clark and Loop subdivision with edge and vertex creases, boundary interpolation modes and face-varying UV interpolation modes, following the subdivision rules of the OpenUSD specification. | T1 | 4 |
| RB-GEOM-007 | Adaptive tessellation | Subdivision and displaced meshes tessellate to a target edge length in pixels or scene units, up to a maximum level, without cracks between patches. Geometry outside the view frustum tessellates at a lower, configurable rate. | T1 | 4 |
| RB-GEOM-008 | Dicing camera | A fixed camera can drive tessellation decisions so the tessellated surface stays identical across an animated shot. | T2 | 4 |
| RB-GEOM-009 | Displacement | Scalar displacement along the normal and vector displacement in tangent or object space, with scale and zero level, on subdivision and linear meshes. The bounds padding is user set; when displaced points exceed it the renderer warns with the measured required padding. | T1 | 4 |
| RB-GEOM-010 | Automatic bump from displacement | Displacement detail finer than the tessellation rate is applied as bump, selectable per ray type, so fine detail survives at moderate tessellation. | T2 | 4 |

## Curves

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-011 | Curves | Linear, cubic Bezier, B-spline and Catmull-Rom curves with per-vertex width, rendered as camera-facing ribbons, normal-oriented ribbons or round tubes. | T1 | 4 |
| RB-GEOM-012 | Curve attributes | Curves carry root UVs, per-curve and per-vertex attributes and a strand identifier available to shading for per-strand variation. | T1 | 4 |
| RB-GEOM-013 | Minimum pixel width | Curves thinner than a minimum pixel width are widened and their opacity reduced proportionally, keeping thin hair and grass free of aliasing at low sample counts. | T2 | 4 |

## Points and splats

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-014 | Points | Point sets render as spheres, camera-facing disks or oriented disks with per-point radius, attributes and motion samples or velocities. | T1 | 4 |
| RB-GEOM-015 | Gaussian splats | Sets of 3D Gaussians (position, scale and rotation, opacity, spherical harmonic color up to degree 3) load from PLY or OpenUSD and are ray traced as ellipsoid kernels, composited in depth order with all other geometry. Splats appear in reflections and cast shadows when their visibility flags allow. | T2 | 5 |
| RB-GEOM-016 | Splat relighting | Splats render either with their captured radiance or in a relit mode that derives a diffuse albedo and responds to scene lights; computer-generated objects cast shadows onto splats in both modes. | T3 | 5 |

## Analytic primitives

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-017 | Analytic primitives | Spheres, disks, cylinders, cones, boxes and planes are intersected analytically without tessellation and carry UVs and normals. | T3 | 4 |
| RB-GEOM-018 | Implicit surfaces | Signed-distance functions supplied by a procedural plug-in are rendered by sphere tracing with a bounded step count. | T3 | 5 |

## Instancing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-019 | Instancing | An instance references a prototype with its own transform and shares the prototype's acceleration structure. Instances can override material, visibility and attributes; instances of instances nest to a bounded depth. | T1 | 4 |
| RB-GEOM-020 | Point instancers | Arrays of instances are created from point data: positions, orientations, scales, prototype indices, identifiers, masks and velocities. Per-instance memory stays a small fixed size so scenes with hundreds of millions of instances fit in memory. | T1 | 4 |
| RB-GEOM-021 | Per-instance variation | Instance identifier, a stable random value and per-instance attributes are readable in shading networks, so one prototype renders with varied color or texture. | T1 | 4 |

## Stand-ins and procedurals

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-022 | Stand-ins | A stand-in references an external OpenUSD stage, render package or Alembic file with a bounding box. It loads when a ray first enters its bounds or at render start, per setting, and can be unloaded under memory pressure and reloaded later. | T1 | 4 |
| RB-GEOM-023 | Procedural generators | Procedural plug-ins expand into prims at render time from parameters, with deterministic output and bounded prim and memory counts enforced by the host. | T2 | 5 |
| RB-GEOM-024 | Stand-in preview | Stand-ins display in the render view and in host viewports as a bounding box, a point sample or a proxy mesh, chosen per stand-in. | T2 | 4 |
| RB-GEOM-025 | Level of detail | A prim with several representations (OpenUSD variants or level-of-detail attributes) uses the one selected by its projected screen size, with a per-setup bias. | T3 | 5 |

## Robustness and precision

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-026 | Degenerate geometry | Zero-area faces, non-finite positions and out-of-range indices are dropped and counted per prim in the validation report; they never stop the render. | T1 | 4 |
| RB-GEOM-027 | Large coordinates | Geometry is transformed relative to the camera before conversion to single precision, so georeferenced models far from the origin render without precision artifacts. | T1 | 4 |
| RB-GEOM-028 | Self-intersection avoidance | Secondary rays start from offset positions computed from bounded floating-point error, so surfaces render without shadow acne and without a user-tuned bias. | T1 | 4 |

## Exact surfaces

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-GEOM-029 | NURBS surfaces | Rational and non-rational NURBS surfaces with trim loops render from their exact definition. The renderer evaluates them itself, either by direct ray intersection or by adaptive tessellation refined until the deviation from the exact surface is below a sub-pixel tolerance, chosen per setup. Patches and trims join without cracks. | T3 | 5 |
| RB-GEOM-030 | Subdivision limit surfaces | The limit surface of a subdivision mesh is evaluated as exact patches or tessellated to a sub-pixel tolerance, so silhouettes, positions and normals match the limit surface at any view distance, including under displacement. | T3 | 5 |
| RB-GEOM-031 | Tolerance control and statistics | The deviation tolerance is set per setup and per prim in pixels or scene units. Statistics report the largest deviation and the evaluation method per prim, and results are deterministic under RB-PATH-015. | T3 | 5 |
