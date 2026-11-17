# Material editor and look-development environment

This area covers authoring materials in the node editor and parameter panel, previewing them, and
the look-development stage used to judge materials under controlled lighting.

Domain objects: material editor, preview, material variant, look-development stage, turntable,
reference image.

## Material editor

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LOOKDEV-001 | Node editor | Shading networks are edited as node graphs: add from a searchable palette, connect typed sockets, group, collapse and edit parameters inline. Every edit is an undoable command. | T1 | 4 |
| RB-LOOKDEV-002 | Parameter panel | OpenPBR parameters appear grouped by layer with ranges, units, color pickers and texture slots; editing in the panel and in the graph stay in sync. | T1 | 4 |
| RB-LOOKDEV-003 | Material preview | A progressive preview renders the material on a chosen preview object and environment, updating with every edit. | T1 | 4 |
| RB-LOOKDEV-004 | Node thumbnails | Nodes show small rendered thumbnails of their output, computed at low priority. | T2 | 4 |
| RB-LOOKDEV-005 | Inline diagnostics | Validation errors and warnings appear on the nodes they concern. | T1 | 4 |
| RB-LOOKDEV-006 | Material variants | A material holds alternative parameter sets switchable per look, such as color options of one product. | T2 | 4 |
| RB-LOOKDEV-007 | MaterialX text view | The material's MaterialX document can be viewed and edited as text, with round trip to the graph and errors reported by line. | T2 | 4 |
| RB-LOOKDEV-008 | Assignment by drag and drop | Materials drop onto prims in the render view or outliner, creating a look rule for the target. | T1 | 4 |

## Look-development stage

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LOOKDEV-009 | Look-development stage | A built-in stage with studio, neutral and outdoor environments, a shadow-catcher floor, a color chart and gray and chrome reference spheres lights the asset independently of its scene lighting. | T1 | 4 |
| RB-LOOKDEV-010 | Turntable | The asset or environment rotates over a set number of frames and renders as a sequence from the stage. | T1 | 4 |
| RB-LOOKDEV-011 | Environment adjustment | Environment rotation, exposure and background blur adjust live in the stage. | T1 | 4 |
| RB-LOOKDEV-012 | Reference comparison | A reference photograph displays side by side or overlaid with the render under the same display transform. | T2 | 4 |
| RB-LOOKDEV-013 | Chart validation | A rendered color chart is compared with its reference values and the color difference per patch is reported. | T3 | 4 |
