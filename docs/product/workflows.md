# Reference workflows

These end-to-end workflows are the acceptance scenarios for RenderBuilder. Each step names the
capabilities it exercises; each workflow ends with observable acceptance results.

## W1. Look development of an asset with a turntable

1. Open a new render document and add an OpenUSD asset stage as scene source (RB-SCENE-001,
   RB-SCENE-002, RB-IO-001).
2. Load the look-development stage with a studio environment, shadow-catcher floor and reference
   spheres (RB-LOOKDEV-009, RB-LIGHT-008, RB-SURF-017).
3. Create OpenPBR materials in the node editor; connect albedo, roughness, metalness and normal
   textures with UDIM tokens (RB-LOOKDEV-001, RB-SURF-001, RB-TEX-001, RB-TEX-002, RB-NODE-016).
4. Assign materials by dragging them onto prims, which creates look rules (RB-LOOKDEV-008,
   RB-OVR-001).
5. Tune in the render view; store snapshots of two variants and compare them (RB-VIEW-001,
   RB-VIEW-014, RB-VIEW-015, RB-LOOKDEV-006).
6. Set a final preset with adaptive sampling and denoising, and a turntable of 120 frames
   (RB-SCENE-022, RB-NOISE-002, RB-NOISE-008, RB-LOOKDEV-010).
7. Export the look as a MaterialX look document (RB-OVR-004, RB-IO-008).

**Acceptance**

- Untiled textures are converted in the background and the second render reuses the cache entries
  (RB-TEX-009).
- The A/B comparison shows both variants with the same display transform.
- The 120 frames are written as half-float OpenEXR with beauty, albedo and normal outputs
  (RB-OUT-012).
- Reopening the exported look in a new document on the same asset reproduces the same image
  bit for bit (RB-PATH-015).

## W2. Interior architectural still from a live ArchBuilder model

1. In ArchBuilder, switch a 3D view to path-traced viewport mode, which starts a session and sends
   an Arc Scene snapshot (RB-HOST-001, RB-HOST-008).
2. Open the session in RenderBuilder as a live-link document (RB-HOST-011).
3. Apply the interior daylight with artificial lighting scenario; portals are generated from the
   windows (RB-STUDY-003, RB-STUDY-004, RB-LIGHT-011).
4. Set the site date and time; the physical sun and sky follow (RB-STUDY-001, RB-LIGHT-010).
5. Assign architectural glazing to every element whose IFC class is a window, using a selector on
   host attributes (RB-HOST-015, RB-SCENE-017, RB-SURF-019).
6. Put sun, sky and fixtures in three light groups; the fixtures use photometric profiles
   (RB-LIGHT-019, RB-LIGHT-009).
7. Move a wall in ArchBuilder while the render view is running (RB-HOST-003).
8. Render the final image with two-point perspective, denoise it and balance the groups in the
   light mixer; commit the mix to the lights (RB-CAM-021, RB-NOISE-008, RB-IMGR-006).
9. Return the image to ArchBuilder and place it on a sheet (RB-HOST-009).

**Acceptance**

- The wall change appears in the render view without restarting the session, and no frame from
  the earlier revision is displayed after the change.
- Light group outputs plus emission and background sum to beauty (RB-OUT-006).
- After committing the mix, a fresh render matches the mixed image within noise tolerance.
- Terminating the RenderBuilder process during rendering leaves ArchBuilder running and a
  restarted session resumes the same view (RB-HOST-005).

## W3. Illuminance study of an office floor

1. Open an exported Arc Scene package of a building as scene source (RB-IO-003).
2. Create sensor grids at 0.8 m above the floors of all office rooms with 0.5 m spacing
   (RB-STUDY-005).
3. Enable the physically valid mode and the interior artificial scenario with photometric fixtures
   (RB-STUDY-007, RB-STUDY-003, RB-LIGHT-013).
4. Render a false-color illuminance image with a logarithmic legend (RB-STUDY-006).
5. Repeat under a CIE overcast sky for daylight factor (RB-STUDY-002, RB-STUDY-009).
6. Export the study report (RB-STUDY-008).

**Acceptance**

- Enabling the physically valid mode with a light multiplier other than one produces a validation
  error naming the light.
- The report lists minimum, average, maximum and uniformity per room as CSV and JSON.
- Output metadata records that the physically valid mode was on.
- The study's validation cases pass within the documented tolerance (RB-STUDY-010).

## W4. Solar study and panorama tour for a design review

1. From an ArchBuilder session, set the site location and true north (RB-STUDY-001).
2. Render a solar study on the solstices from 08:00 to 18:00 every hour, shadows only on a white
   model (RB-STUDY-012, RB-OVR-011).
3. Render a sun-hour map for the courtyard over March (RB-STUDY-013).
4. Render a set of four stereo panoramas with shared exposure (RB-STUDY-015, RB-STUDY-017,
   RB-CAM-008).
5. Publish the results to Arc Hub (RB-BATCH-021).

**Acceptance**

- The solar study writes 22 frames whose shadow directions match the computed sun positions.
- The panorama tour opens in the Arc Hub web viewer with all four positions linked.
- The four panoramas share one exposure value, recorded in their metadata.

## W5. Character with skin and hair from MeshBuilder

1. From MeshBuilder, send a character with subdivision surfaces, displacement and curves to a
   session (RB-HOST-001, RB-GEOM-006, RB-GEOM-009, RB-GEOM-011).
2. Shade skin with random-walk subsurface and a coat, eyes with transmission and a separate iris
   (RB-SURF-005, RB-SURF-006, RB-SURF-004).
3. Shade hair with melanin-based color and per-strand variation (RB-STYL-001, RB-STYL-002,
   RB-STYL-004).
4. Light with an environment and a key rectangle light; split hair and skin with light path
   expressions (RB-LIGHT-004, RB-OUT-004, RB-OUT-005).
5. Lock tessellation with a dicing camera and render the shot with motion blur (RB-GEOM-008,
   RB-CAM-016, RB-PATH-016).

**Acceptance**

- The displaced surface shows no cracks between patches, and no padding warning is logged.
- The skin and hair outputs from light path expressions sum to the beauty of those objects.
- Tessellation statistics are identical across all frames of the shot.

## W6. Volume shot with motion blur

1. Add an OpenVDB cache with density, temperature and velocity grids as a volume prim
   (RB-VOL-001, RB-IO-011).
2. Shade it with scattering, blackbody emission from temperature and a density remap ramp
   (RB-VOL-008, RB-VOL-009, RB-VOL-010).
3. Enable velocity motion blur and a height-falloff atmosphere (RB-VOL-003, RB-VOL-005).
4. Write a custom volume output from the volume shader (RB-VOL-013).
5. Render the sequence through the CLI on a frame range (RB-BATCH-001).

**Acceptance**

- The custom volume output is present in every frame and is zero outside the volume.
- Changing the volume quality scale changes noise but not the mean image beyond the statistical
  tolerance.
- The CLI exits with the success code and the statistics report peak memory by category
  (RB-DIAG-002).

## W7. Compositing hand-off

1. Define outputs: beauty, lighting components, light groups, depth, position, normal, motion
   vectors and Cryptomatte object and material layers (RB-OUT-001, RB-OUT-002, RB-OUT-006,
   RB-OUT-009).
2. Use closest-sample filtering for data outputs and Gaussian filtering for color
   (RB-OUT-007).
3. Write a multipart OpenEXR with DWAB for color parts and ZIP for data parts (RB-OUT-012).
4. Enable component denoising and write denoising auxiliaries (RB-NOISE-009, RB-NOISE-012).
5. Define render passes so the foreground and background render as separate holdout passes
   (RB-OVR-012, RB-SCENE-014).

**Acceptance**

- The lighting components sum to beauty within the documented differences (RB-OUT-003).
- Cryptomatte manifests list the stable prim paths and a compositing tool extracts a matte per
  object.
- Data outputs contain no blended values across object edges.
- Each part uses the compression it was assigned.

## W8. Animation on the self-hosted farm

1. Validate the document and package it with all dependencies (RB-BATCH-005, RB-BATCH-006,
   RB-SCENE-019).
2. Submit the package to the Arc Hub render service; a preflight estimates time and memory per
   frame (RB-BATCH-020).
3. Workers fetch the package, verify digests, render frame packets with a per-frame time limit and
   upload outputs (RB-BATCH-015, RB-BATCH-017, RB-BATCH-014, RB-NOISE-004).
4. A frame fails on one worker for lack of memory and is retried on a larger worker
   (RB-BATCH-019).
5. One hero frame renders as 16 tiles across workers and is merged (RB-BATCH-012).
6. The sequence denoises temporally (RB-NOISE-011).

**Acceptance**

- No frame starts on a worker whose package digests do not match.
- The merged hero frame is bit-identical to a single-process render of the same frame.
- The failed frame is reported with the out-of-memory category and succeeds on retry.
- Results appear in Arc Hub as a version with thumbnails and setup metadata.

## W9. Product visualization from SolidBuilder

1. From SolidBuilder, send an assembly to a session; the host tessellates it to the requested
   tolerance (RB-HOST-001).
2. Apply library appearance templates for anodized metal, plastic and glass; link them so library
   updates are reported (RB-LIB-007, RB-LIB-003, RB-LIB-004).
3. Add a studio light rig from the library and rounded edges on machined parts (RB-LIGHT-027,
   RB-NODE-017).
4. Create three color variants and render each (RB-LOOKDEV-006, RB-OVR-001).
5. Render an exploded view with transform overrides and a turntable (RB-OVR-010,
   RB-LOOKDEV-010).

**Acceptance**

- Each variant renders from the same document without duplicating materials.
- The exploded view does not modify the SolidBuilder assembly.
- Changing a linked library item marks the document's item as out of date until updated.

## W10. OpenUSD and MaterialX scene on GPU and CPU

1. Render an OpenUSD stage with MaterialX looks from the CLI, selecting a render settings prim
   (RB-BATCH-002, RB-IO-001, RB-IO-008).
2. Validate the scene for the GPU device; review features it does not support
   (RB-SCENE-024, RB-PATH-020).
3. Render on a GPU from a vendor of choice (RB-PATH-019, RB-BATCH-009).
4. Run the parity report against the CPU reference (RB-PATH-021).

**Acceptance**

- Validation lists every unsupported feature with prim paths, and with the fallback policy set to
  fail, rendering does not start.
- With supported features only, the parity report passes for every output.
- The same command succeeds on GPUs from at least two vendors.
