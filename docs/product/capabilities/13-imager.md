# Imagers and post-processing

This area covers imagers: ordered image operations applied after sampling to progressive and final
images, including relighting with the light mixer.

Domain objects: imager chain, imager, light mix, burn-in.

## Chain and operations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-IMGR-001 | Imager chain | An ordered list of imagers applies per setup or per output to progressive and final images. Editing an imager re-runs the chain on the accumulated samples without restarting the render. | T1 | 4 |
| RB-IMGR-002 | Target outputs | Each chain names the outputs it applies to; scene-referred OpenEXR outputs stay untouched unless explicitly targeted. | T1 | 4 |
| RB-IMGR-003 | Exposure and white balance | Exposure in stops and white balance by temperature and tint or by a picked neutral pixel. | T1 | 4 |
| RB-IMGR-004 | Tone curve | An optional highlight-compression curve applies before the view transform of the color configuration. | T1 | 4 |
| RB-IMGR-005 | Color correction | Lift, gamma and gain, saturation, contrast, hue shift and RGB and luminance curves. | T1 | 4 |
| RB-IMGR-006 | Light mixer | Light group intensity, color and on-off state change after rendering, rebuilding beauty from group outputs instantly. A mix can be committed back to the lights as one undoable command. | T1 | 4 |
| RB-IMGR-007 | Glare and bloom | Diffraction glare computed from the camera aperture shape, streaks and bloom with threshold and chromatic dispersion; the effect conserves image energy. | T2 | 4 |
| RB-IMGR-008 | Lens effects | Post vignetting and lateral chromatic aberration with strength controls. | T2 | 4 |
| RB-IMGR-009 | Film grain | Luminance-dependent grain with size and strength, seeded per frame so results are reproducible. | T3 | 4 |
| RB-IMGR-010 | Background compositing | The image composites over a color, image or plate using the render's alpha and shadow-catcher outputs. | T2 | 4 |
| RB-IMGR-011 | Burn-ins | Text overlays for frame, camera, setup, date and project write only to display outputs for review. | T2 | 4 |
| RB-IMGR-012 | Run mode | Each imager runs progressively or once at the end of the render, per setting. | T2 | 4 |
| RB-IMGR-013 | Network imager | A shading network evaluated per pixel on output channel inputs (no lighting) writes beauty or a new output. | T3 | 5 |
| RB-IMGR-014 | Plug-in imagers | Imagers supplied as sandboxed plug-ins run in the chain, for example a user-provided image model for stylization or upscaling; frame-to-frame consistency is the plug-in's declared property. | T3 | 5 |
