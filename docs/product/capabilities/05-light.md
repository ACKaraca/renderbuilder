# Lights and light filters

This area covers light types, photometric units, light linking and groups, light filters and
reusable light rigs.

Domain objects: light, light shape, photometric profile, sky model, portal, light filter, light
group, light link, light rig.

## Light types

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIGHT-001 | Point and sphere lights | Point lights with a radius emit from a sphere surface and cast soft shadows; radius zero gives a true point light. | T1 | 4 |
| RB-LIGHT-002 | Spot lights | Cone angle and penumbra falloff shape the beam; a radius softens shadows. | T1 | 4 |
| RB-LIGHT-003 | Distant lights | Parallel light with an angular diameter for soft sun shadows; intensity is specified as illuminance at normal incidence. | T1 | 4 |
| RB-LIGHT-004 | Rectangle lights | Rectangular emitters with width, height, optional image texture and a spread control that narrows emission toward the normal. | T1 | 4 |
| RB-LIGHT-005 | Disk lights | Circular emitters with radius, texture and spread. | T1 | 4 |
| RB-LIGHT-006 | Cylinder lights | Tube emitters with length and radius, emitting from the side surface. | T2 | 4 |
| RB-LIGHT-007 | Mesh lights | Any mesh emits light with textured or per-vertex emission and is sampled by area and emitted power. Instanced mesh lights share data. | T1 | 4 |
| RB-LIGHT-008 | Environment light | A high-dynamic-range image in latitude-longitude, angular or cube layout lights the scene from infinity with rotation and is importance sampled. Its camera visibility is controlled separately from its lighting. | T1 | 4 |
| RB-LIGHT-009 | Photometric profiles | IES LM-63 and EULUMDAT profiles apply to point, spot, area and mesh lights with profile orientation and optional normalization to the profile's luminous flux. | T1 | 4 |
| RB-LIGHT-010 | Physical sun and sky | A physically based sky model and sun disk are driven by sun direction or by location, date and time, with turbidity and ground albedo. Sun and sky intensities are physically consistent with each other. | T1 | 4 |
| RB-LIGHT-011 | Light portals | Rectangle portals placed in openings guide environment and sky sampling into interiors without blocking or adding light. | T2 | 4 |
| RB-LIGHT-012 | Emissive materials as lights | Material emission above a power threshold is registered automatically in many-light sampling, so emissive geometry lights rooms without explicit mesh lights. | T2 | 4 |

## Units and color

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIGHT-013 | Light units | Intensity is set in watts, lumens, candela, lux (distant lights), nits (area lights) or unitless scale. Conversions are documented and shown in the light panel; area normalization keeps intensity independent of size when enabled. | T1 | 4 |
| RB-LIGHT-014 | Color temperature | Light color can be set as a blackbody temperature in kelvin with a tint, multiplied with an optional color. | T1 | 4 |
| RB-LIGHT-015 | Exposure control | Each light has an exposure parameter in stops applied on top of its intensity. | T1 | 4 |
| RB-LIGHT-016 | Contribution multipliers | Per-light multipliers for diffuse, specular, transmission, subsurface, volume and indirect contribution. Non-unit multipliers mark the render as non-physical in metadata. | T2 | 4 |

## Linking, groups and visibility

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIGHT-017 | Light linking | Each light includes or excludes collections for illumination; the default is the whole scene. Links survive source reloads through stable identity. | T1 | 4 |
| RB-LIGHT-018 | Shadow linking | Each light names the collections that cast shadows from it, independent of which prims it illuminates. | T2 | 4 |
| RB-LIGHT-019 | Light groups | Lights belong to named groups; each group can write its own beauty and per-lobe outputs for relighting in the light mixer or compositing. | T1 | 4 |
| RB-LIGHT-020 | Light visibility | Per-light switches control visibility to camera and reflections, shadow casting and volume contribution. | T1 | 4 |
| RB-LIGHT-021 | Many lights at constant cost | Scenes with thousands to millions of lights, including instanced emitters, render without per-light setup and with sampling cost that grows slowly with light count. | T1 | 4 |

## Light filters

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIGHT-022 | Barn doors | Four flaps with individual angles, edge softness and rotation shape a light's beam. | T2 | 4 |
| RB-LIGHT-023 | Projected textures | A texture is projected through a light with scale, offset, rotation and density, for window patterns and foliage shadows. | T2 | 4 |
| RB-LIGHT-024 | Distance falloff | Near and far ranges with ramps fade light in and out with distance, in addition to physical falloff. | T2 | 4 |
| RB-LIGHT-025 | Light blockers | Box, sphere, plane and cylinder blockers attenuate light inside their volume with density and edge softness, applied to illumination, shadows or both. | T2 | 4 |
| RB-LIGHT-026 | Filter stacking | Several filters apply to one light in a defined order, and one filter can be shared by several lights. | T2 | 4 |

## Light rigs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIGHT-027 | Light rigs | A set of lights and filters with relative transforms saves as a rig in a library and is placed as a unit, parented to a chosen prim or the camera. | T2 | 4 |
| RB-LIGHT-028 | Light state per setup | Each render setup can enable, disable or scale lights without changing the lights themselves, so day and night setups share one scene. | T1 | 4 |
