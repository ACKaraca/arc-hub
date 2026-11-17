# Production tracking: entities, tasks and pipeline

The configurable data model of production tracking for film, animation, games, visualization
and any project managed as entities and tasks: entity types and fields, pipeline steps, tasks,
status lists, saved views and published outputs.

Domain objects:

- **Entity type** and **entity**: built-in or custom kinds of tracked things (shots, assets) and their records.
- **Field definition**: a typed field of an entity type, including calculated and summary fields.
- **Pipeline step**: a stage of work such as modeling, rigging or lighting.
- **Task**: a unit of work on an entity at a pipeline step, with assignees, dates and status.
- **Task template**: the tasks and dependencies created for a new entity.
- **Status list**: the ordered statuses available to an entity type.
- **Page**: a saved view of entities with filters, grouping and layout.
- **Published output**: a document version registered with production context.

## Entities and fields

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PROD-001 | Built-in entity types | Projects use episodes, sequences, shots and assets, with the hierarchy episode, sequence, shot optional per project. Shots carry cut in and out, frame range, handles, frame rate and status; assets carry type and subtype from project lists. | T2 | 4 |
| HB-PROD-002 | Custom entity types | Administrators define custom entity types with name, icon, fields and permissions, scoped to projects or to the organization; up to 100 custom types per organization. | T2 | 4 |
| HB-PROD-003 | Field types | Fields are text, number, decimal, percent, currency, duration, date, date-time, checkbox, list, status, entity link (single or multiple), user, URL, image, color, timecode or frame range, each with required, default and validation settings. | T2 | 4 |
| HB-PROD-004 | Calculated and summary fields | Formula fields compute from fields of the same entity (arithmetic, text, dates, conditions); summary fields aggregate linked entities (count, sum, minimum, maximum, share in a status, latest). Both recompute on change, and definitions that would create cycles are rejected. | T2 | 4 |
| HB-PROD-005 | Entity links | Entities link many-to-many, for example assets used in shots, and links can carry their own fields. | T2 | 4 |
| HB-PROD-006 | Import and export | Entities import from CSV and XLSX to create or update by a matching key, with a preview. Shot lists import from OpenTimelineIO and CMX 3600 edit decision lists, creating shots with cut information. Every view exports as CSV. | T2 | 4 |

## Tasks and pipeline

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PROD-007 | Pipeline steps | Each project keeps an ordered list of pipeline steps with name, short code, color, applicable entity types and department. | T2 | 4 |
| HB-PROD-008 | Task templates | Per entity type, task templates define tasks with steps, default durations and dependencies; applying a template creates the tasks, and applying it again adds only missing tasks. | T2 | 4 |
| HB-PROD-009 | Tasks | A task belongs to an entity and a step and carries assignees (users or groups), status, start and due dates, duration, estimate, logged effort, dependencies, priority and notes. | T2 | 4 |
| HB-PROD-010 | Status lists | Projects define statuses with code, name, color and category (waiting, ready, in progress, review, final, on hold, omitted); each entity type uses a subset with a default. | T2 | 4 |
| HB-PROD-011 | Status propagation | Rules move downstream tasks from waiting to ready when their upstream tasks reach a final status and roll entity status up from task statuses. Rules are automation rules (HB-JOB-013) shipped as editable defaults. | T2 | 4 |
| HB-PROD-012 | My tasks | Each user sees their tasks across projects and starts work from a task, which opens the matching Arc application with the task context. | T2 | 4 |
| HB-PROD-013 | Generic task tracking | Tasks, steps and status lists are available in building, infrastructure and product projects for tracking deliverables, not only in media projects. | T2 | 4 |

## Views

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PROD-014 | Pages | Pages show entities as lists, thumbnail grids, boards grouped by status or Gantt charts (HB-PLAN-001), with nested AND/OR filters including linked-field paths and relative dates, grouping, sorting, summary rows and conditional formatting. Pages are personal or shared and can be part of project templates. | T2 | 4 |
| HB-PROD-015 | Entity detail | Each entity has a detail page with fields, tasks, submissions, notes, published outputs and history. | T2 | 4 |
| HB-PROD-016 | Inline and bulk editing | Users edit fields inline with immediate validation and update many selected entities at once. | T2 | 4 |

## Published outputs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PROD-017 | Published outputs | Publishing registers a document version with production context (entity, task, step, output type, frame range, color space) and links to the upstream outputs it was made from, forming a dependency graph. | T2 | 4 |
| HB-PROD-018 | Path templates | Projects define path templates with tokens (for example entity, step and zero-padded version) and storage roots per operating system; Arc applications and the sync agent derive working file locations and names from them, and publishing validates against them. | T2 | 4 |
| HB-PROD-019 | Task context in Arc applications | MeshBuilder, RenderBuilder and other Arc applications open a task with its working files and latest upstream outputs, and publishing from the application registers outputs and creates a submission (HB-MEDIA-001). | T2 | 4 |
| HB-PROD-020 | Upstream updates | Applications list loaded upstream outputs, detect newer published versions and update selected references. | T2 | 4 |
| HB-PROD-021 | Scripts in other applications | The publish and load operations are available through the REST API and the Python client so studios can script them inside other content-creation applications. | T3 | 4 |
