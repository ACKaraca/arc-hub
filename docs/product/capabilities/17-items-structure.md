# Product data: items, structures and reference operations

Items and their revisions, bills of materials, where-used analysis, copy design and the reference
operations (rename, move, replace) that keep assemblies and drawings consistent.

Domain objects:

- **Item**: a master record of a part, assembly, document or purchased component.
- **Item revision**: a released or working state of an item with frozen properties and structure.
- **BOM line**: a parent item revision, child item, quantity, unit, position and source.
- **Item link**: alternate, substitute, related item or attachment.
- **Copy design plan**: a tree of documents with copy, reuse, replace or exclude decisions.

## Items

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ITEM-001 | Items | Items carry a number from a numbering scheme, title, category, unit of measure, lifecycle, revision and properties, and exist independently of files. | T1 | 3 |
| HB-ITEM-002 | Items from documents | Assigning an item to a document creates or links an item using mapped properties. Each item revision has one primary document and any number of secondary documents such as drawings. | T1 | 3 |
| HB-ITEM-003 | Item update | When a linked document receives a new version, the item is marked out of date; updating, manually or by an automatic job, refreshes its properties and structure from the document. | T1 | 3 |
| HB-ITEM-004 | Item release | Item revisions are released through their lifecycle or a change order; a released revision freezes properties, structure and the linked document versions. | T1 | 3 |
| HB-ITEM-005 | Item links | Items link to alternates, substitutes, related items and attachments; links are shown on both ends. | T2 | 3 |
| HB-ITEM-006 | Item import | Items import from CSV to create or update, with a preview and per-row errors. | T2 | 3 |
| HB-ITEM-007 | Item comparison | Two revisions of an item compare by properties and structure. | T2 | 3 |
| HB-ITEM-008 | Copy items with structure | An item and its structure are duplicated for a variant, with new numbers from the numbering scheme and a link to the source. | T2 | 3 |

## Bills of materials

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ITEM-009 | BOM extraction | A job extracts the structure of assembly documents (SolidBuilder assemblies, STEP assembly structure): components, quantities, units, positions and reference designators, each line keeping its source document. | T1 | 3 |
| HB-ITEM-010 | BOM views | Structures are shown multi-level, parts-only with rolled-up quantities, or first level only, with columns chosen from item properties. | T1 | 3 |
| HB-ITEM-011 | BOM editing | Users add lines for items not present in the model (fasteners, coatings, packaging) and override quantities with a reason; manual lines and overrides are kept and labeled through later updates from the document. | T1 | 3 |
| HB-ITEM-012 | BOM comparison | Two revisions, or an item and its current document, compare as added, removed, quantity changed and property changed lines. | T1 | 3 |
| HB-ITEM-013 | BOM export | Structures export as CSV, XLSX and JSON with a documented schema using saved column templates; publication to enterprise systems is described in HB-ECO-012. | T1 | 3 |
| HB-ITEM-014 | Rollups | Structures compute totals of numeric properties such as mass and cost, and counts per item, through all levels. | T2 | 3 |

## Where-used

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ITEM-015 | Uses and where-used | For documents and items, users see direct children and parents or the full recursive tree, filtered by type and state, at latest or at a chosen revision, including drawings that reference a model. | T1 | 3 |
| HB-ITEM-016 | Impact analysis | Before a change, users list every parent up to top-level assemblies with their states and open change orders, and export the list. | T1 | 3 |

## Reference operations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ITEM-017 | Copy design | Users select a top-level assembly or drawing and see the tree of dependents; per node they choose copy, reuse, replace with another document or exclude, set naming rules (prefix, suffix, replace text or numbering scheme) and a destination folder, and preview the result. A job creates the new documents, rewrites references in Arc documents and in DWG external reference paths, starts each copy at the initial lifecycle state and links it to its source. | T1 | 3 |
| HB-ITEM-018 | Rename with reference repair | Renaming a document updates the documents that reference it by creating new versions with corrected references, where the format allows. The rename is refused while any referencing document is checked out by another user. | T1 | 3 |
| HB-ITEM-019 | Move with reference repair | Moving documents whose references are path-based repairs referencing documents in the same way as renaming. | T1 | 3 |
| HB-ITEM-020 | Replace component | Users replace one component with another across selected parents, for example a fastener, creating new versions of the parents. | T2 | 3 |
| HB-ITEM-021 | Pack and unpack | A document with all its references exports as a ZIP with relative paths and a manifest; importing a pack can preserve the original identifiers when they are free. | T2 | 3 |
| HB-ITEM-022 | Delete with dependency check | Deleting a document or item that others use is refused with the list of users of it; making it obsolete through its lifecycle is offered instead. | T1 | 3 |
