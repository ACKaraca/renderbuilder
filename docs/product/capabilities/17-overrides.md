# Scene overrides and look assignment

This area covers how materials and attributes are assigned to scene prims without modifying the
scene sources, and how render-time overrides and passes are layered per setup.

Domain objects: look, assignment rule, override layer, override operation, render pass.

## Look assignment

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OVR-001 | Looks | A look is a named, ordered set of rules that assign materials, displacement and attribute values to collections. A document holds several looks; each setup selects one. | T1 | 4 |
| RB-OVR-002 | Rule precedence | Rules apply in order and later rules win over earlier ones for the same attribute; looks apply over the assignments carried by the scene sources. | T1 | 4 |
| RB-OVR-003 | Assignment provenance | For any prim the renderer lists the source assignment and every rule and override that changed it, in order. | T1 | 4 |
| RB-OVR-004 | Look exchange | Looks import from and export to MaterialX look documents and OpenUSD material bindings. | T1 | 4 |
| RB-OVR-005 | Attribute rules | Rules set visibility flags, subdivision level, displacement, light links, holdout and primvars as well as materials. | T1 | 4 |
| RB-OVR-006 | Rule diagnostics | Rules that match no prim, and rules fully overridden by later rules, are reported. | T1 | 4 |

## Override layers

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OVR-007 | Override layers | An override layer is an ordered list of operations evaluated over the scene at render time; setups enable layers in order. Sources are never modified. | T1 | 4 |
| RB-OVR-008 | Enable and disable | Operations hide or remove prims and lights from the render by collection. | T1 | 4 |
| RB-OVR-009 | Parameter overrides | Operations set or scale material and light parameters by collection and parameter name, for example halving all lights of one group. | T2 | 4 |
| RB-OVR-010 | Transform overrides | Operations replace or offset prim transforms, for exploded views and alternative layouts. | T2 | 4 |
| RB-OVR-011 | Global material override | All materials are replaced by one material except for chosen collections, for clay and white-model renders that keep glazing and emitters. | T1 | 4 |

## Passes and variants

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-OVR-012 | Render passes | A setup defines passes, each with collections set to visible, hidden, holdout or shadow-only, rendered as separate outputs for compositing. | T2 | 4 |
| RB-OVR-013 | Variant selection | A setup selects OpenUSD variants and host design options, so alternatives render from one document. | T2 | 4 |
