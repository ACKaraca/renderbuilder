# Glossary

Terms as used in the RenderBuilder documentation, one line each.

| Term | Meaning |
|---|---|
| Adaptive sampling | Stopping pixels whose estimated error is below a threshold while others keep sampling. |
| Arc Scene | The suite's renderable scene contract (instances, materials, lights, cameras, settings) and its session protocol, defined in `arc-scene`. |
| Assignment rule | An entry of a look that assigns a material or attribute values to a collection. |
| Auxiliary buffer | An output such as albedo or normal that guides denoising. |
| Beauty | The final lit color output of a render. |
| Closure | The result of evaluating a material at a point: a weighted set of scattering lobes. |
| Collection | A named set of prims defined by explicit members and/or a selector expression. |
| Contour | A stylized line drawn at discontinuities of identity, normal or depth. |
| Cryptomatte | An open standard for encoding identifier mattes in OpenEXR files. |
| Deep image | An image storing a list of depth-ordered samples per pixel. |
| Delta | An incremental scene change keyed by stable identifiers and tagged with a revision. |
| Device | The hardware back end that runs the integrator: CPU or GPU. |
| Dicing camera | A camera used only to decide tessellation rates. |
| Feature matrix | The declared list of shading and scene features a device supports. |
| Film | The set of accumulation buffers for all outputs of a render. |
| Furnace test | A test scene with uniform white illumination that checks a material neither gains nor loses energy beyond its definition. |
| Holdout | A prim that cuts a transparent hole in color outputs while still occluding. |
| Imager | A post-processing operation applied to accumulated images. |
| Integrator | The algorithm that estimates light transport along paths. |
| Light group | A named set of lights whose contribution is written to its own outputs. |
| Light hierarchy | The bounding volume hierarchy over lights used for many-light sampling. |
| Light linking | Restricting which prims a light illuminates or which prims cast its shadows. |
| Light mixer | Post-render adjustment of light group intensity and color. |
| Light path expression | A regular expression over scattering events that defines an output. |
| Look | A named, ordered set of assignment rules. |
| Look-development stage | A controlled lighting environment for judging materials. |
| Loss report | A list of content that an import could not represent. |
| MaterialX | An open standard for material node graphs and their exchange. |
| Multiple importance sampling | Combining several sampling strategies with weights that minimize variance. |
| Next-event estimation | Sampling a light directly from a path vertex and testing visibility with a shadow ray. |
| OpenPBR | An open specification of a physically based surface material model. |
| Output | A named image channel written by a render, with its filter, type and file target. |
| Override layer | An ordered list of render-time operations over collections that never modifies sources. |
| Parity test | A statistical image comparison between two devices rendering the same scene. |
| Path guiding | Sampling directions from learned distributions of incident light. |
| Physically valid mode | A mode that disables non-physical controls for studies. |
| Portal | A rectangle in an opening that guides environment sampling into an interior. |
| Prim | A node of the render scene graph with a stable identity and a kind. |
| Primvar | A typed attribute on a prim with an interpolation mode, readable by shading. |
| Render document | A `.renderb` Arc document holding setups, looks, overrides and references to scene sources. |
| Render package | A self-contained snapshot of a resolved render scene with all assets, used for farms and reproduction. |
| Render pass | A setup subdivision rendering chosen collections as visible, hidden, holdout or shadow-only. |
| Render scene | The derived, in-memory scene the engine renders. |
| Render setup | A named combination of camera, resolution, look, overrides, settings and outputs. |
| Rendering space | The scene-linear color space in which light transport is computed. |
| Revision | A monotonically increasing number identifying a scene state in a session. |
| Scene source | An Arc Scene package, host session, OpenUSD stage or render package referenced by a render document. |
| Selector expression | A query that matches prims by path, kind, attributes, material or collection. |
| Sensor grid | A set of points at which illuminance is computed for a study. |
| Session | A connection between a host or job driver and a RenderBuilder session process. |
| Shading program | The compiled intermediate representation of a material shared by all devices. |
| Shadow catcher | A surface that shows only received shadows and reflections, for compositing over photographs. |
| Snapshot (image) | A stored render image with its settings digest, used for comparison. |
| Snapshot (scene) | The complete scene a host sends when a session opens. |
| Stable identity | The source-provided identifier of a prim that survives reloads. |
| Stand-in | A prim that references external content loaded on demand. |
| Study | A render that answers a design question: illuminance, daylight factor, solar sequence, sun hours, panorama set or walkthrough. |
| Texture cache | The memory-bounded store of texture tiles at the mip levels lookups need. |
| Worker | A CLI process that renders jobs from Arc Hub or another queue manager. |
