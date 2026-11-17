# Hair and stylized shading

This area covers the physically based hair and fur model and the non-photorealistic shading used
for stylized animation and technical presentation. Volume shading is specified with volumes in
[04-volume.md](04-volume.md).

Domain objects: hair material, tone band, contour rule, stroke pattern.

## Hair and fur

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STYL-001 | Hair scattering model | A physically based fiber model with primary reflection, transmission and internal reflection lobes plus a residual term, with longitudinal and azimuthal roughness, cuticle tilt and index of refraction. | T2 | 4 |
| RB-STYL-002 | Melanin pigmentation | Hair color derives from eumelanin and pheomelanin concentrations, with per-strand randomization and an optional dye color. | T2 | 4 |
| RB-STYL-003 | Dense hair scattering | Additional bounded depth and an approximation of multiple scattering keep blonde and light hair from looking dark without unbounded path depth. | T2 | 4 |
| RB-STYL-004 | Per-strand variation | Root color, strand identifier and random values from curve attributes drive hair parameters per strand. | T2 | 4 |
| RB-STYL-005 | Fur medulla | A diffuse medulla component models animal fur. | T3 | 5 |

## Stylized shading

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STYL-006 | Toon shading | Banded diffuse shading with up to eight tone bands, each with a threshold, color and optional output; a banded specular highlight; illumination comes from scene lights. | T2 | 4 |
| RB-STYL-007 | Rim lighting | A view-dependent rim from one designated light with width and softness, written to its own output. | T3 | 4 |
| RB-STYL-008 | Contour lines | Outlines are detected from object identity, material identity, normal angle and depth discontinuities, with width in pixels and color per object, for inner and outer lines. Lines are computed from sample data and match between devices. | T2 | 4 |
| RB-STYL-009 | Technical illustration | Presets combine monochrome or flat shading with contour lines and hidden-line styles for presentation renders of building and product models. | T2 | 4 |
| RB-STYL-010 | Stroke patterns | Hierarchical hatching and stroke patterns are driven by shaded luminance and textures to imitate drawn media. | T3 | 5 |
| RB-STYL-011 | Lit color as input | A node converts the lit result of a closure to a color that further nodes remap, for custom ramp-based shading. | T3 | 5 |
