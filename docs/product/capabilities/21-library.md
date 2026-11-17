# Material and appearance libraries

This area covers libraries of materials, lights and environments: their package format,
appearance templates for building materials, linking and versioning, and sharing through Arc Hub.

Domain objects: library, library item, appearance template, library package, pinned version.

## Libraries

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIB-001 | Material libraries | User, project and shared libraries are browsable with thumbnails, tags, categories and search. | T1 | 4 |
| RB-LIB-002 | Library package format | A library is a folder or archive of MaterialX documents, textures, thumbnails and a JSON manifest; the format is documented and readable without RenderBuilder. | T1 | 4 |
| RB-LIB-003 | Linked and copied use | A library item is either copied into the document or linked; a linked item records its version and digest and changes only when the user updates it. | T1 | 4 |
| RB-LIB-004 | Update report | Documents list linked items for which a newer library version exists, with the differences in parameters. | T2 | 4 |
| RB-LIB-005 | Library import | Materials import into a library from MaterialX, OpenUSD and glTF files, with a loss report per item. | T1 | 4 |
| RB-LIB-006 | Thumbnail generation | Thumbnails render in batch on a standard preview object and environment. | T2 | 4 |

## Appearance templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIB-007 | Appearance templates | Parameterized templates for glazing, metal, painted wall, wood, stone, masonry, concrete, ceramic, plastic, water, mirror, metallic paint and fabric expose a few meaningful parameters and generate OpenPBR networks with real-world texture scale. | T1 | 4 |
| RB-LIB-008 | Physical data linkage | A library item can reference host physical material data (density, thermal properties) by identifier; RenderBuilder keeps only the appearance and preserves the reference through round trips. | T2 | 4 |

## Other assets and sharing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| RB-LIB-009 | Light and environment libraries | Photometric profiles, light rigs and high-dynamic-range environments are library items with previews. | T2 | 4 |
| RB-LIB-010 | Hub libraries | Libraries publish to and subscribe from Arc Hub with Hub permissions and versions. | T2 | 4 |
| RB-LIB-011 | Starter content | RenderBuilder ships a starter library of materials, environments and profiles, each item with a recorded origin and license. | T2 | 4 |
| RB-LIB-012 | Entourage assets | People, vegetation and vehicles are library items delivered as instanced stand-ins or splat sets. | T3 | 5 |
