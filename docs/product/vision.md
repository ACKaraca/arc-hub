# Vision

Why Arc Hub exists, who it serves, the principles that shape it, what it deliberately does not do,
and what sets it apart.

## Problem and purpose

Design work is shared work. Architects, engineers, contractors, manufacturers and studios exchange
models, drawings, product structures and media every day, and they need one place where each file
has a history, each decision has a record and each participant sees exactly what they should.
Today that place is usually a hosted service that the customer cannot run, inspect or leave with
all of its data, priced per seat or per call, separate for building projects, product data and
media production, and reachable from desktop tools only through add-ins.

Arc Hub is a self-hostable collaboration and data-management server for design projects. It is a
common data environment, a product data management system, a production tracker and an automation
platform in one Rust server that an organization runs on its own hardware or in its own cloud
account. Every Arc application connects to it natively, and any other tool connects through open
APIs and standards.

## Primary users and their deliverables

| User | Deliverables in Arc Hub |
|---|---|
| Information and document managers | Project setup, naming standards, container states, transmittals, registers |
| Architects and engineers | Versioned models and drawings, reviewed and approved deliverables, resolved issues |
| BIM and coordination managers | Model sets, clash and check results, coordination issues, design packages |
| Contractors and site teams | Answered RFIs, reviewed submittals, completed forms, photo records |
| Mechanical and manufacturing engineers | Released parts and assemblies, items and BOMs, change orders |
| Producers, coordinators and supervisors | Shot and asset tracking, schedules, reviewed and approved submissions |
| Artists and designers | Published outputs from tasks, notes addressed and resolved |
| Clients and external reviewers | Decisions and comments on shared deliverables |
| IT administrators and integrators | A running, backed-up, upgraded server; automations and integrations |

## Product principles

1. **Self-hosting is the default.** One binary, PostgreSQL and a blob store make a complete
   installation; nothing calls home.
2. **Every file has a history, every decision a record.** Versions are immutable; decisions,
   transmittals and audit records are never edited.
3. **One server for every kind of design project.** Building and infrastructure projects,
   products and media productions share accounts, permissions, files, records and automation.
4. **One model of permissions, explained.** Users can always see why someone can or cannot see
   something.
5. **Open by construction.** openCDE and BCF APIs, IFC and IDS, OpenID Connect and SCIM, CloudEvents
   webhooks and a complete documented export.
6. **Arc applications do the format work.** The server runs headless Arc applications for viewing,
   checking, converting and automating, so the server and the desktop agree.
7. **Work is never lost.** Conflicts keep both sides; deletions go to trash; backups are
   consistent by design.
8. **No metering.** Limits exist to protect the server, not to charge for use.

## Scope

The five modules and the platform underneath them:

- **Documents**: organizations, projects, members and permissions; folders, files and versions;
  viewing and comparison; markups; issues; reviews and approvals; transmittals and packages;
  ISO 19650 information management; search; desktop synchronization; construction-phase records.
- **Design collaboration**: design packages between teams, cross-organization sharing,
  multi-user editing sessions with element borrowing, branches and merges, model sets, automatic
  clash runs and model checking.
- **Product data**: check-in and check-out, versions and revisions, lifecycles, categories and
  properties, numbering, items and bills of materials, where-used, copy design, change orders and
  enterprise-system publication.
- **Production tracking**: entities, tasks and pipeline steps, published outputs, submissions,
  playlists and review, notes, scheduling, resource planning and time logs, client review.
- **Platform**: REST API, authentication and identity, webhooks, automation jobs running headless
  Arc applications, model derivatives and property services, granular data queries, data
  exchange between applications, administration, audit and reporting, deployment and operations,
  integrations and optional assistance.

## Non-goals

| Non-goal | Reason |
|---|---|
| Native mobile applications | The web client works on tablet browsers; maintaining native mobile apps is out of proportion for a self-hosted server. Offline field work is covered by Arc applications on laptops. |
| Authoring models in the browser | Authoring belongs to Arc desktop applications; the browser views, reviews and administers. |
| Viewing proprietary formats without published specifications | The suite never reverse engineers formats; such files are stored and versioned, and shared as open-format exports. |
| Estimating, bid management and procurement | These are commercial systems with their own data; Arc Hub stores their documents and links quantities and cost lines at a light level. |
| Full accounting, payments and payroll | Cost tracking stays at budget and commitment level and exchanges data with accounting systems. |
| Facility operations twins with live sensor data | A different product category; models and records are exported to such systems. |
| Photogrammetry and reality-capture processing | Processing belongs to specialized tools; resulting point clouds and meshes are stored and viewed. |
| Converting live-action footage into CG elements | Specialized media processing outside collaboration. |
| On-set camera ingest and dailies streaming | Specialized media infrastructure; submissions start from files. |
| Pipeline integrations shipped inside other vendors' applications | Arc Hub provides the API and Python client; studios script other applications themselves. |
| Embedding custom web pages or scripts in the web client | Running third-party code in the client conflicts with its security model; custom actions open external pages instead. |
| An application marketplace | Integrations are installed by administrators from their own sources. |
| Multi-master replication between sites | Remote sites use cache nodes; organizations that need separate servers federate shares between them. |
| Per-call or per-seat metering in the software | Contrary to the product principles. |

## Positioning

Arc Hub covers the professional scope of common data environments such as Bentley ProjectWise,
Trimble Connect, Oracle Aconex and Autodesk Docs, of product data management systems such as
SOLIDWORKS PDM, PTC Windchill and Siemens Teamcenter, and of production trackers such as ftrack,
Kitsu and Autodesk Flow Production Tracking.

## Differentiators

| Differentiator | Capabilities |
|---|---|
| Self-hosting as the default deployment, with a complete documented export and no outbound connections | HB-OPS-001, HB-OPS-008, HB-ADMIN-012, HB-ORG-004 |
| One server for building, infrastructure, product and media projects | HB-ORG-007, HB-ORG-008, HB-PROD-013 |
| Open APIs and standard identity: openCDE Foundation, BCF and Documents APIs, OpenID Connect, SAML, SCIM | HB-API-021, HB-ISSUE-021, HB-API-022, HB-ACCESS-002, HB-ACCESS-006 |
| No metering of API calls, conversions or storage | HB-API-006, HB-ORG-005 |
| Branch and merge for model data, with journal-exact comparison | HB-SESSION-014, HB-SESSION-015, HB-VIEW-024 |
| Element-level multi-user editing that never discards work | HB-SESSION-004, HB-SESSION-008 |
| A headless Arc runner for automation instead of a closed cloud service | HB-JOB-004, HB-JOB-005, HB-JOB-006 |
| Rule-based model checking with IDS alongside clash detection | HB-COORD-012, HB-INFO-020 |
| ISO 19650 states, naming and delivery plans as first-class data | HB-INFO-007, HB-INFO-013, HB-INFO-021 |
| One permission model with explanations across all modules | HB-ACCESS-017, HB-ACCESS-020, HB-ACCESS-023 |
| Tamper-evident audit trail | HB-ADMIN-001, HB-ADMIN-002 |
| Desktop work that tolerates disconnection and preserves conflicts | HB-SYNC-004, HB-SYNC-011, HB-PDM-007 |
| Content and element search filtered by permissions | HB-SEARCH-002, HB-SEARCH-003, HB-SEARCH-007 |
| Agent access through the Model Context Protocol with explicit boundaries | HB-ASSIST-003, HB-ASSIST-004 |
