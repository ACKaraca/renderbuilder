# Cameras

This area covers camera projections, lens optics, exposure and shutter behavior, and the camera
utilities used by visualization and production work.

Domain objects: camera, projection, lens, aperture, shutter, clipping plane.

## Projections

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-CAM-001 | Perspective camera | Defined by focal length and film back, or by field of view, with film offset (lens shift), near and far clipping and a fit mode (fill, horizontal, vertical, overscan) for aspect mismatches. Changing resolution aspect never changes the framed horizontal or vertical extent chosen by the fit mode. | T1 | 4 |
| RB-CAM-002 | Orthographic camera | Parallel projection defined by a view width in scene units and the same fit modes as the perspective camera. | T1 | 4 |
| RB-CAM-003 | Spherical panorama | Equirectangular projection over a full 360 by 180 degree range or a partial longitude and latitude range, with the horizon at the image center row. | T1 | 4 |
| RB-CAM-004 | Cube map | Renders six 90-degree faces into one image in a strip or cross layout, or into six files, with seam-free face edges. | T2 | 4 |
| RB-CAM-005 | Fisheye | Equidistant, equisolid, orthographic and stereographic mappings with a field of view up to 360 degrees; pixels outside the image circle are black with zero alpha. | T2 | 4 |
| RB-CAM-006 | Cylindrical panorama | Horizontal field of view up to 360 degrees with either a projective or an angular vertical mapping. | T2 | 4 |
| RB-CAM-007 | Stereo pair | Left and right eyes with interaxial distance and convergence distance using off-axis projection. Eyes render as separate files or as views of one multi-view OpenEXR file. | T2 | 4 |
| RB-CAM-008 | Omnidirectional stereo | Stereo spherical panoramas for headsets with interpupillary distance and smooth pole merging that reduces separation toward the zenith and nadir; output in top-bottom or side-by-side layout. | T2 | 4 |
| RB-CAM-009 | Half-sphere stereo | Stereo 180-degree panoramas in side-by-side layout for forward-facing immersive viewing. | T3 | 4 |

## Optics

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-CAM-010 | Depth of field | Thin-lens model driven by focus distance and f-number, or aperture radius in scene units, consistent with focal length. Focus distance can follow a picked point or a prim. | T1 | 4 |
| RB-CAM-011 | Aperture shape | Blade count, rotation, blade curvature, anamorphic squeeze and an optional image mask define the aperture, which shapes out-of-focus highlights. | T2 | 4 |
| RB-CAM-012 | Lens distortion | Radial and tangential polynomial distortion, or an ST map image, distort camera rays. An overscan option renders the extra border needed to undistort in compositing. | T2 | 4 |
| RB-CAM-013 | Focal plane tilt | The plane of focus can be tilted around two axes for tilt-shift photography looks without moving the image plane. | T3 | 5 |
| RB-CAM-014 | Physical lens systems | A sequence of spherical and aspheric lens elements with materials is traced for each camera ray, producing natural vignetting, distortion and aberration. | T3 | 5 |

## Exposure and shutter

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-CAM-015 | Physical exposure | Exposure is set from ISO, shutter time and f-number or as an exposure value directly. It scales image values before imagers and, unless explicitly linked, does not change depth of field or motion blur. | T1 | 4 |
| RB-CAM-016 | Shutter interval | The shutter opens and closes relative to the frame (centered, start, end or custom offsets), with an exposure curve over the open interval that weights time samples. | T1 | 4 |
| RB-CAM-017 | Rolling shutter | Each image row or column samples a different time offset with selectable direction and readout duration. | T3 | 4 |
| RB-CAM-018 | Camera motion blur | Camera transform motion samples blur the whole image consistently with object motion blur in the same shutter interval. | T1 | 4 |

## Camera utilities

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-CAM-019 | Multiple cameras per setup | One render setup renders several cameras in one job; each camera writes its own outputs through the `<camera>` file template token. | T2 | 4 |
| RB-CAM-020 | Clipping planes and section boxes | User clipping planes and section boxes cut geometry for camera rays, optionally with a colored cap on the cut faces of closed solids; lighting is computed with or without the cut part per setting. | T2 | 4 |
| RB-CAM-021 | Two-point perspective | Verticals stay vertical by computing the lens shift that keeps the image plane parallel to the up axis while the camera aims up or down. | T2 | 4 |
| RB-CAM-022 | Overscan | A border of configurable width renders outside the display window and is stored in the data window for post-processing and distortion. | T2 | 4 |
