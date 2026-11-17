# Surface shading

This area covers the surface material models: OpenPBR surface as the primary model, interchange
surface models, layering, compositing surfaces and special-purpose materials.

Domain objects: material, surface model, closure, layer stack, shadow catcher.

## OpenPBR surface

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SURF-001 | OpenPBR surface | The full OpenPBR surface parameter set (base, specular, metalness, transmission, subsurface, coat, fuzz, thin film, emission, geometry) is supported with the semantics and defaults of the specification version recorded in the document. | T1 | 4 |
| RB-SURF-002 | Base layer | Energy-preserving rough diffuse and a metallic conductor with edge tint; metalness blends between dielectric and conductor responses. | T1 | 4 |
| RB-SURF-003 | Specular reflection | Microfacet reflection with multiple-scattering energy compensation, anisotropy with rotation, index of refraction, weight and color. | T1 | 4 |
| RB-SURF-004 | Transmission | Rough dielectric refraction with transmission color reached at a given depth, interior scattering, dispersion from an Abbe number and a thin-walled mode for sheets. | T1 | 4 |
| RB-SURF-005 | Subsurface scattering | Random-walk subsurface scattering with per-channel radius, scale and anisotropy; thin-walled objects use translucency instead. | T1 | 4 |
| RB-SURF-006 | Coat | A coat layer with weight, absorption color, roughness, index of refraction, anisotropy, darkening of the base and its own normal input. | T1 | 4 |
| RB-SURF-007 | Fuzz | A retro-reflective fuzz layer for cloth and dusty surfaces with weight, color and roughness. | T1 | 4 |
| RB-SURF-008 | Thin film | Thin-film interference with thickness and index of refraction alters specular color; in spectral mode it is evaluated per wavelength. | T2 | 4 |
| RB-SURF-009 | Emission | Emission specified in nits with color; emissive surfaces are sampled as lights per RB-LIGHT-012. | T1 | 4 |
| RB-SURF-010 | Opacity and cutouts | Geometric opacity makes surfaces partially present, including in shadows; a cutout mode thresholds opacity for leaves and fences without noise. | T1 | 4 |
| RB-SURF-011 | Energy conservation | Every lobe combination returns no more energy than it receives in a white furnace test within a stated tolerance; the furnace scenes ship with the test corpus. | T1 | 4 |

## Interchange and simple surfaces

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SURF-012 | Interchange surface models | UsdPreviewSurface and glTF metallic-roughness materials, including supported glTF material extensions, render from their own definitions instead of an approximate conversion. | T1 | 4 |
| RB-SURF-013 | MaterialX standard surface | The MaterialX standard surface node renders through its MaterialX definition. | T2 | 4 |
| RB-SURF-014 | Simple surfaces | Diffuse-only, unlit constant color and emission-only surfaces for diagrams, previews and light cards. | T1 | 4 |
| RB-SURF-015 | Layering and mixing | Materials stack vertically with masks, so a coat of dust or water sits over a base with physically coupled energy, or mix horizontally as weighted blends of closures. | T1 | 4 |
| RB-SURF-016 | Two-sided materials | A surface uses different materials on its front and back faces. | T2 | 4 |

## Compositing surfaces

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SURF-017 | Shadow catcher | A shadow-catcher surface is invisible except for the shadows, reflections and indirect light it receives, written to alpha and a shadow output for placing renders on photographs. | T1 | 4 |
| RB-SURF-018 | Backplate projection | A shadow catcher can carry a camera-projected background plate so reflections of the ground match the photograph. | T2 | 4 |

## Special-purpose materials

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-SURF-019 | Architectural glazing | Thin-walled glass passes colored, physically correct light through windows, including in shadow rays, without caustic noise and without double refraction offsets. | T1 | 4 |
| RB-SURF-020 | Flake paint | A flake layer with density, size, orientation spread and color between base and coat produces metallic and pearlescent paints. | T2 | 4 |
| RB-SURF-021 | Measured materials | Tabulated measured reflectance data supplied in a documented format renders as a surface closure. | T3 | 5 |
