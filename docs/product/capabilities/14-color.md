# Color management

This area covers color configuration, the rendering space, texture and output color spaces, and
display transforms.

Domain objects: color configuration, color space, display, view, look.

## Configuration and transforms

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-COLOR-001 | Color configuration | An OpenColorIO configuration governs all color: a built-in ACES 2 based configuration by default, or a custom configuration set per document or per user. The configuration's identity is recorded in every output. | T1 | 4 |
| RB-COLOR-002 | Rendering space | Rendering happens in a scene-linear space chosen from the configuration (ACEScg by default, linear Rec.709 available); every input color converts to it on load. | T1 | 4 |
| RB-COLOR-003 | Texture color spaces | Each texture carries a color space; file rules from the configuration assign defaults by name and type, and non-color data maps are read raw. | T1 | 4 |
| RB-COLOR-004 | Display and view transforms | The render view and display-referred outputs apply a chosen display, view and look; switching them never re-renders. | T1 | 4 |
| RB-COLOR-005 | Looks and grades | Configuration looks and color decision lists apply in the view and to display outputs. | T2 | 4 |
| RB-COLOR-006 | Color entry | Color fields accept scene-linear values, display-referred values and hexadecimal display codes, converting exactly between them. | T1 | 4 |
| RB-COLOR-007 | Output color spaces | Each output declares its color space; OpenEXR files store chromaticities and the color space name. | T1 | 4 |
| RB-COLOR-008 | Exact final transforms | Final outputs use exact color processors, not lookup-table approximations, so CPU and GPU renders produce the same display images. | T2 | 4 |
| RB-COLOR-009 | Gamut warning | The render view highlights pixels outside the display gamut. | T3 | 4 |
| RB-COLOR-010 | Spectral-ready colors | Colors keep their source color space tag through the scene so spectral upsampling in spectral mode starts from the authored values. | T3 | 5 |
