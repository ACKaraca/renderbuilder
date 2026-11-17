# Architectural studies

This area covers rendering for building design decisions: daylight and sun, illuminance and
luminance studies, solar studies, panoramas and walkthroughs, using the same engine as production
renders.

Domain objects: site, study, lighting scenario, sensor grid, legend, panorama set, camera path.

## Daylight and sun

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STUDY-001 | Site and sun position | Sun position derives from the document's geographic location (latitude, longitude, elevation, time zone and true north from the coordinate reference record) and a date and time, accurate to within 0.01 degree. | T1 | 4 |
| RB-STUDY-002 | Sky models | A physically based clear sky, the CIE standard general skies (overcast, intermediate and clear) and measured high-dynamic-range skies, each with a matching sun. | T1 | 4 |
| RB-STUDY-003 | Lighting scenarios | Presets for exterior daylight, interior daylight, interior artificial, interior daylight with artificial, and night set which lights are enabled, the sky, portals and exposure defaults. | T1 | 4 |
| RB-STUDY-004 | Automatic portals | Portals are generated from host glazing elements such as windows and curtain walls. | T2 | 4 |

## Illuminance and luminance

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STUDY-005 | Sensor grids | Grids of sensors lie on surfaces or on work planes at a height above floors, generated from host rooms or spaces with a spacing; results are in lux or footcandles. | T1 | 4 |
| RB-STUDY-006 | False-color renders | Visible surfaces render as false-color illuminance or luminance with a legend, linear or logarithmic scale and a chosen range. | T1 | 4 |
| RB-STUDY-007 | Physically valid mode | Disables every non-physical control (contribution multipliers, clamps, light linking, artistic decay) and requires photometric units; the output metadata states that the mode was on. | T1 | 4 |
| RB-STUDY-008 | Study report | Per grid and per room: minimum, average and maximum values and uniformity ratios, exported as CSV, JSON and an image with legend. | T2 | 4 |
| RB-STUDY-009 | Daylight factor | Daylight factor per sensor under a CIE overcast sky. | T2 | 4 |
| RB-STUDY-010 | Validated results | Illuminance results agree with analytical solutions and published lighting test cases within a stated tolerance; the cases run in continuous integration. | T2 | 4 |
| RB-STUDY-011 | Glare evaluation | Fisheye luminance renders from viewpoints compute a daylight glare metric. | T3 | 5 |

## Solar studies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STUDY-012 | Solar study sequences | Images render over a time range at an interval (within a day, or across selected dates), with an option to render shadows only over a neutral model. | T1 | 4 |
| RB-STUDY-013 | Sun-hour maps | Hours of direct sun per surface point over a period render as a false-color map with legend. | T2 | 4 |
| RB-STUDY-014 | Sun path overlay | The sun path and analemma for the site draw over the image or the view. | T3 | 4 |

## Panoramas and walkthroughs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-STUDY-015 | Panorama sets | Several mono or stereo panoramas with positions render with consistent exposure and export with a tour description that Arc Review and the Arc Hub web viewer open. | T2 | 4 |
| RB-STUDY-016 | Walkthrough paths | A camera follows a host path or keyframed path at a set speed with eye height and look-ahead, rendering a sequence. | T2 | 4 |
| RB-STUDY-017 | Shared exposure | A set of views uses one automatically determined exposure, so images of the same project compare fairly. | T2 | 4 |
