# Shading utilities and node networks

This area covers the shading network model, its compilation, and the utility nodes for math,
color, context, surface detail and custom outputs.

Domain objects: shading network, node, node definition, node group, socket, connection, shading
program.

## Network model

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NODE-001 | Shading networks | A material is a directed acyclic graph of typed nodes ending in surface, displacement and volume terminals. Socket types are float, integer, boolean, color, vector, point, normal, matrix, string and closure; allowed implicit conversions are listed, and graphs with loops are rejected. | T1 | 4 |
| RB-NODE-002 | Standard node library | The MaterialX standard and physically based node libraries form the base node set, so networks exchange with other MaterialX tools without translation. | T1 | 4 |
| RB-NODE-003 | Node groups | A subgraph becomes a reusable node group with exposed inputs and outputs, stored with a version in a library or document. | T1 | 4 |
| RB-NODE-004 | Network validation | Type mismatches, missing inputs, unknown nodes and nodes the chosen device does not support are reported with the node path before rendering. | T1 | 4 |
| RB-NODE-005 | Network compilation | Networks compile to the shading program representation with constant folding and removal of unused branches. Compiled programs are cached by network hash, so unchanged materials never recompile. | T1 | 4 |
| RB-NODE-006 | Exposed parameters | A material can expose parameters that looks and objects set individually, so one network serves many variants without duplication. | T1 | 4 |

## Math and conversion

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NODE-007 | Math nodes | Arithmetic, trigonometry, power and logarithm, minimum, maximum, clamp, mix, smoothstep, range remapping, comparison and selection, plus vector operations (dot, cross, normalize, length, transform between spaces). | T1 | 4 |
| RB-NODE-008 | Color nodes | Hue, saturation and value adjustment, contrast, gamma, color correction, luminance, channel split, combine and shuffle, and color space conversion through the color configuration. | T1 | 4 |
| RB-NODE-009 | Ramps and curves | Float and color ramps with linear, smooth and constant interpolation, and curve remapping of values. | T1 | 4 |
| RB-NODE-010 | Switch nodes | Integer switches, conditionals and front-back switches choose between inputs. | T1 | 4 |

## Context nodes

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NODE-011 | Geometric state | Position, shading and geometric normal, tangent, UV, ray direction, facing ratio and derivatives, in object, world, camera or normalized device space. | T1 | 4 |
| RB-NODE-012 | Attribute readers | Nodes read named primvars of any type with a fallback value when the attribute is absent. | T1 | 4 |
| RB-NODE-013 | Object and instance info | Object identifier, instance identifier, name hash and stable random values per object, instance, face or strand. | T1 | 4 |
| RB-NODE-014 | Ray-type switch | A node returns different values per ray type (camera, shadow, diffuse, specular, transmission) or by path depth, for example a cheaper material in reflections. | T2 | 4 |

## Surface detail

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NODE-015 | Bump | A height input perturbs the shading normal with a scale; bump nodes chain with each other and with normal maps. | T1 | 4 |
| RB-NODE-016 | Normal maps | Tangent-space (MikkTSpace) and object-space normal maps with per-channel flip and strength. | T1 | 4 |
| RB-NODE-017 | Rounded edges | Sharp edges render rounded with a radius by ray-traced normal blending, without changing geometry. | T2 | 4 |
| RB-NODE-018 | Occlusion and curvature | Ray-traced ambient occlusion and curvature nodes with bounded sample counts and maximum distance, for wear and dirt masks. | T2 | 4 |
| RB-NODE-019 | Projections | Planar, cylindrical, spherical, box and triplanar projections with blending, projection from a named camera, and UV transforms. | T1 | 4 |
| RB-NODE-020 | Nearest-point sampling | A node finds points of a point set or point grid near the shading point and returns their count, mean distance or averaged attribute. | T3 | 5 |

## Custom outputs and languages

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-NODE-021 | Output write nodes | Any color, float or vector value in a network writes to a named output, for custom mattes and utility passes. | T1 | 4 |
| RB-NODE-022 | OSL shader nodes | Compiled OSL shaders within the supported subset act as nodes in a network; shaders outside the subset are rejected with the unsupported operations listed. | T2 | 5 |
