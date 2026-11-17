# Volumes

This area covers participating media: sparse grid volumes, homogeneous and atmospheric media, the
volume shader and volume outputs.

Domain objects: volume prim, grid, medium, atmosphere, volume shader.

## Volume primitives

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VOL-001 | Sparse grid volumes | Volume prims read named OpenVDB and NanoVDB grids (float, half and vector) with their grid-to-world transforms; several grids per prim feed one volume shader. | T1 | 4 |
| RB-VOL-002 | Volume level of detail | Mip-mapped grid levels, read from file or generated on load, are selected by ray footprint with a per-prim bias. A CLI command writes the levels into a new grid file. | T2 | 5 |
| RB-VOL-003 | Velocity motion blur | Velocity grids blur volumes with a velocity scale, frames-per-second reference and an outlier threshold that limits extreme velocities. | T2 | 4 |
| RB-VOL-004 | Mesh-bounded media | A closed mesh can bound a homogeneous or textured medium, used for fog boxes, liquids and the interior of colored glass. | T1 | 4 |
| RB-VOL-005 | Atmosphere | A global medium with uniform or height-falloff density affects camera rays and lights; each light can be excluded from atmospheric scattering. | T1 | 4 |
| RB-VOL-006 | Automatic stepping | Volumes are integrated with tracking against local density bounds instead of a user step size; a quality scale trades noise for speed. | T1 | 4 |
| RB-VOL-007 | Nested media | Overlapping dielectrics and media are resolved by priority, so ice in water or liquid in glass renders with the correct interfaces. | T2 | 4 |

## Volume shading

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VOL-008 | Volume shader | Density, scattering color, absorption color, emission color and intensity, and a phase function (Henyey-Greenstein or a two-lobe blend) define the medium. Inputs accept grid values and shading network outputs. | T1 | 4 |
| RB-VOL-009 | Blackbody emission | A temperature grid drives physically based blackbody emission with an intensity scale and temperature remapping. | T2 | 4 |
| RB-VOL-010 | Grid remapping | Shading networks remap grid values with ramps, curves and math, for example density shaping or color from temperature. | T1 | 4 |
| RB-VOL-011 | Multiple scattering approximation | Dense media such as clouds can use an approximation of high-order scattering beyond the volume depth limit, with a strength control. | T3 | 5 |
| RB-VOL-012 | Emissive volumes as lights | Emissive volumes join many-light sampling so they illuminate nearby surfaces without noise from indirect hits alone. | T3 | 5 |

## Volume outputs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-VOL-013 | Volume outputs | Custom outputs written inside a volume shader are integrated along the ray with transmittance weighting; deep output includes volume segments as samples. | T2 | 4 |
