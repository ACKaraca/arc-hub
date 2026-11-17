# Vision

Why Arc Hub exists, who it serves, the principles that shape it, what it deliberately does not do,
and what sets it apart.

## Problem and purpose

Design work is shared work. Architects, engineers, contractors, manufacturers and studios exchange
models, drawings, product structures and media every day, and they need one place where each file
has a history, each decision has a record and each participant sees exactly what they should.
Today that place is usually a hosted service that the customer cannot run, inspect or leave with
all of its data, priced per seat or per call, separate for building projects, product data and
media production, and reachable from desktop tools only through add-ins and from the field only through a browser.

Arc Hub is a self-hostable collaboration and data-management server for design projects. It is a
common data environment, a product data management system, a production tracker and an automation
platform in one Rust server that an organization runs on its own hardware or in its own cloud
account. Every Arc application connects to it natively, native mobile clients serve field work, and
any other tool connects through open APIs, standards and connector-based integrations.

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
| Field staff and inspectors | Offline sheets and models, completed checklists, photos and issues from a phone or tablet |
| Estimators and procurement staff | Estimates from takeoff quantities, bid comparisons, purchase orders linked to cost lines |
| IT administrators and integrators | A running, backed-up, upgraded server; automations, extensions and integrations |

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
  ISO 19650 information management; search; desktop synchronization; construction-phase records,
  estimating, bid management and procurement.
- **Design collaboration**: design packages between teams, cross-organization sharing,
  multi-user editing sessions with element borrowing, branches and merges, model sets, automatic
  clash runs and model checking.
- **Product data**: check-in and check-out, versions and revisions, lifecycles, categories and
  properties, numbering, items and bills of materials, where-used, copy design, change orders and
  enterprise-system publication.
- **Production tracking**: entities, tasks and pipeline steps, published outputs, submissions,
  playlists and review, notes, scheduling, resource planning and time logs, client review.
- **Platform**: REST API, authentication and identity, webhooks, automation jobs running headless
  Arc applications, model derivatives and property services, granular data queries, live data
  streams linked to model elements, data exchange between applications, administration, audit and
  reporting, deployment and operations including multi-site replication, integrations, sandboxed
  extensions with an administrator-curated catalog, and optional assistance.
- **Field and authoring clients**: native mobile clients built from the same Rust crates, with
  offline documents, sheets and models, issues, markups, forms, photos, reviews, push
  notifications and barcode lookup ([ADR-0007](../adr/0007-native-mobile-clients.md)); opening the
  browser build of the owning Arc product for a document, with check-out and session handling;
  a toolkit of connector-based integrations inside content-creation and CAD applications.
- **Proprietary formats**: files in formats without a published specification are viewed through
  Arc exchange packages that connectors produce and through bridge workers that an organization
  licenses itself, as set out in suite [ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md).
- **Working-level commercial records**: estimates from takeoff quantities, bid packages and
  comparison, and purchase orders linked to cost lines.
- **Processing as jobs**: reality-capture processing, media ingest and footage reconstruction run
  open tools as automation jobs; the server performs none of this work itself.

## Non-goals

| Non-goal | Reason |
|---|---|
| Reverse engineering undocumented formats or binaries | A legal and licensing problem. Files in such formats are stored and versioned, viewed through exchange packages from connectors running in the user's own licensed application, or converted by a bridge worker the organization licenses (HB-VIEW-027, HB-JOB-018). |
| Full accounting, payments and payroll | Cost tracking, estimating and procurement stay at the level of budgets, commitments and orders and exchange data with accounting systems. The server moves no money. |
| Payments, prices and revenue sharing in the extension catalog | The catalog lists what administrators approve; it handles no purchases (HB-INTEG-017). |
| Live camera feeds and streaming from capture hardware | Specialized media infrastructure. Files from cameras enter through ingest jobs (HB-JOB-021). |
| Equipment control, building automation and maintenance work management | Another product category. The Hub stores readings and links them to model elements (HB-DATA-015) and exports models and records to such systems. |
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
| Native mobile clients from the same Rust crates, with offline models and sheets | HB-MOBILE-001, HB-MOBILE-003, HB-MOBILE-005 |
| Authoring in the browser under the same check-out and audit rules as the desktop | HB-FILE-028, HB-FILE-029, HB-SESSION-017 |
| Sandboxed extensions with declared permissions and an administrator-curated catalog, no payments | HB-INTEG-015, HB-INTEG-017, HB-INTEG-020 |
| Integrations inside other applications built on suite connectors and the public API | HB-PIPE-001, HB-PIPE-004, HB-PIPE-007 |
| Asynchronous multi-site replication that keeps both sides of every conflict | HB-OPS-024, HB-OPS-025, HB-OPS-026 |
