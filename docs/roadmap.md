# Roadmap

Milestones of Arc Hub mapped onto the suite phases. Each milestone has a goal, a scope stated as
capability rows, dependencies and exit criteria written as acceptance scenarios. There are no
dates; a milestone is complete when its exit criteria are met and verified.

The suite phases are defined in the [arc-suite](https://github.com/ACKaraca/arc-suite) repository.
Arc Hub starts in phase 2.

## Scope rule

Every capability row belongs to exactly one milestone. A milestone's scope is the rows explicitly
listed for it plus, as stated, the remaining rows of its phase.

## M1: Documents core (phase 2)

**Goal**: a self-hosted server where teams store, version, view and discuss files, and Arc
applications open and save through the Hub.

**Scope**

- HB-ORG-001 to HB-ORG-025 (phase-2 rows)
- HB-ACCESS-001 to HB-ACCESS-025 (phase-2 rows)
- HB-FILE-001 to HB-FILE-027 (phase-2 rows)
- HB-VIEW-001 to HB-VIEW-008, HB-VIEW-011
- HB-ISSUE-001 to HB-ISSUE-016, HB-ISSUE-019 to HB-ISSUE-022 (phase-2 rows)
- HB-INFO-001 to HB-INFO-012 (phase-2 rows)
- HB-SYNC-001 to HB-SYNC-006
- HB-API-001 to HB-API-021 (phase-2 rows)
- HB-JOB-001 to HB-JOB-003; HB-DATA-001 to HB-DATA-003, HB-DATA-006, HB-DATA-007
- HB-ADMIN-001 to HB-ADMIN-006, HB-ADMIN-012 to HB-ADMIN-017 (phase-2 rows)
- HB-OPS-001 to HB-OPS-019 (phase-2 rows)
- HB-INTEG-001, HB-INTEG-002, HB-INTEG-009

**Dependencies**

- `arc-core`: `arc-base`, `arc-doc` (package reading), `arc-command` (schemas), `arc-io-bcf`,
  `arc-io-pdf`, `arc-hub-api`.
- Arc Review: command-line tool for derivatives and the WebAssembly viewer build.
- Arc applications: Hub connection in the `arc-app` shell (DraftBuilder first, from suite phase 1).

**Exit criteria**

- Workflow [W1](product/workflows.md#w1-set-up-an-organization-and-a-project-from-a-template)
  passes.
- Workflow [W4](product/workflows.md#w4-resolve-an-issue-between-the-browser-bcf-tools-and-an-arc-application)
  passes, including the BCF round trip.
- Workflow [W5](product/workflows.md#w5-work-on-hub-documents-from-an-arc-application) passes.
- A complete organization export from M1 imports into a fresh server with identifiers preserved.
- The performance targets for metadata reads, list pages and uploads are met on the reference
  node.

## M2: Controlled information exchange (phase 2)

**Goal**: ISO 19650 information management with reviews, approvals, transmittals, markups and
search; the remaining phase-2 platform rows.

**Scope**

- HB-MARKUP-001 to HB-MARKUP-016 (phase-2 rows)
- HB-REVIEW-001 to HB-REVIEW-015 (phase-2 rows)
- HB-TRANS-001 to HB-TRANS-014 (phase-2 rows)
- HB-INFO-013 to HB-INFO-019
- HB-SEARCH-001 to HB-SEARCH-010 (phase-2 rows)
- HB-SYNC-017
- All remaining phase-2 rows of every area, including the phase-2 rows of HB-VIEW-014 to HB-VIEW-023 and the
  phase-2 rows of OPS, ADMIN, API and DATA not listed in M1.

**Dependencies**

- M1.
- ArchBuilder sheet publishing (suite phase 2) for sheet sets with metadata.

**Exit criteria**

- Workflow [W2](product/workflows.md#w2-share-an-information-container-through-iso-19650-states)
  passes.
- Workflow [W3](product/workflows.md#w3-review-drawings-with-markups-and-issue-them-by-transmittal)
  passes.
- Workflow [W14](product/workflows.md#w14-install-back-up-restore-and-upgrade-a-server) passes
  from an M1 server to an M2 server.
- The permission leak test suite passes for search, listings and exports.

## M3: Design collaboration and automation (phase 3)

**Goal**: teams exchange design packages, model sets are coordinated automatically, and
customers automate with headless Arc applications.

**Scope**

- HB-PKG-001 to HB-PKG-012 (phase-3 rows)
- HB-COORD-001 to HB-COORD-014 (phase-3 rows)
- HB-SESSION-014
- HB-JOB-004 to HB-JOB-011, HB-JOB-013, HB-JOB-014
- HB-DATA-004, HB-DATA-005, HB-DATA-008 to HB-DATA-012
- HB-SYNC-007 to HB-SYNC-016
- All remaining phase-3 rows outside the product data areas (PDM, ITEM, ECO).

**Dependencies**

- M2.
- Arc Review clash detection and IDS checking in its command-line tool (suite phase 3).
- `arc-hub-api` support for packages, model sets and jobs; the `apps/arc-hub-sync` binary.

**Exit criteria**

- Workflow [W6](product/workflows.md#w6-share-and-consume-design-packages-between-teams) passes.
- Workflow [W7](product/workflows.md#w7-from-automatic-clash-run-to-closed-issue) passes.
- Workflow [W13](product/workflows.md#w13-event-driven-automation-with-a-headless-arc-job)
  passes.
- A sandbox escape test suite (network, filesystem, resource limits) passes on every supported
  worker platform.

## M4: Product data (phase 3)

**Goal**: controlled release of engineering data with items, structures and change orders.

**Scope**

- HB-PDM-001 to HB-PDM-026 (phase-3 rows)
- HB-ITEM-001 to HB-ITEM-022
- HB-ECO-001 to HB-ECO-014 (phase-3 rows)

**Dependencies**

- M3 (the job system runs transition actions, structure extraction and copy design).
- SolidBuilder command-line tool for structure extraction, property write-back and reference
  rewriting (suite phase 3); DraftBuilder for DWG property mapping.

**Exit criteria**

- Workflow [W9](product/workflows.md#w9-release-a-part-with-lifecycle-and-revision) passes.
- Workflow [W10](product/workflows.md#w10-engineering-change-with-where-used-and-enterprise-publication)
  passes.
- Workflow [W11](product/workflows.md#w11-copy-a-design-for-a-variant) passes.
- Atomic check-in of an assembly with 1,000 parts either creates all versions or none under fault
  injection.

## M5: Production tracking and construction records (phase 4)

**Goal**: media productions and construction phases run on the same server.

**Scope**

- HB-PROD-001 to HB-PROD-021
- HB-MEDIA-001 to HB-MEDIA-016 (phase-4 rows)
- HB-PLAN-001 to HB-PLAN-013 (phase-4 rows)
- HB-SITE-001 to HB-SITE-019 (phase-4 rows)
- All remaining phase-4 rows of every area.

**Dependencies**

- M3.
- MeshBuilder and RenderBuilder task context and publishing (suite phase 4); RenderBuilder farm
  worker for render jobs; `arc-nodes` for rule graphs.

**Exit criteria**

- Workflow [W12](product/workflows.md#w12-shot-production-loop-with-client-review) passes.
- An RFI and a submittal complete their workflows with transmittals, and the logs export.
- The Model Context Protocol endpoint passes its permission test suite with agent boundaries
  enforced.

## M6: Depth and scale (phase 5)

**Goal**: element-level multi-user editing, model merges, federation and optional assistance.

**Scope**

- All phase-5 rows not listed in M7, including HB-SESSION-001 to HB-SESSION-013, HB-SESSION-015, HB-SESSION-016,
  HB-PKG-013, HB-ASSIST-001, HB-ASSIST-002, HB-ASSIST-005 to HB-ASSIST-010, HB-SITE-014 to
  HB-SITE-018, HB-SITE-020.

**Dependencies**

- M5.
- Multi-user support in `arc-doc` change sets and in ArchBuilder and DraftBuilder (suite phase 5).

**Exit criteria**

- Workflow [W8](product/workflows.md#w8-edit-one-model-with-several-people) passes.
- A branch with 500 element changes merges into a main line with 50 concurrent changes, with
  conflicts listed and no change lost.
- A folder share between two Hub servers mirrors versions with verified digests and stops after
  revocation.

## M7: Field work, extensions and integrations (phase 5)

**Goal**: native mobile clients, authoring through the browser build of Arc products, proprietary
formats through exchange packages and licensed bridges, sandboxed extensions, integrations inside
other applications, working-level commercial records, multi-site replication and processing jobs.

**Scope**

- HB-MOBILE-001 to HB-MOBILE-017 ([ADR-0007](adr/0007-native-mobile-clients.md))
- HB-FILE-028 to HB-FILE-031, HB-SESSION-017
- HB-VIEW-027, HB-VIEW-028, HB-JOB-018, HB-JOB-019
- HB-PIPE-001 to HB-PIPE-014
- HB-INTEG-012 to HB-INTEG-021 ([ADR-0008](adr/0008-sandboxed-extensions-and-curated-catalog.md))
- HB-SITE-021 to HB-SITE-031
- HB-OPS-023 to HB-OPS-030 ([ADR-0009](adr/0009-asynchronous-multi-site-replication.md))
- HB-JOB-020 to HB-JOB-022, HB-DATA-015 to HB-DATA-019

**Dependencies**

- M6 for multi-user sessions with browser participants; M5 for production tracking and construction
  records.
- Suite phase 5: browser and mobile builds of Arc products (suite
  [ADR-0018](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0018-web-and-mobile-targets.md)),
  the exchange package and the connectors (suite
  [ADR-0017](https://github.com/ACKaraca/arc-suite/blob/main/docs/adr/0017-exchange-connectors-and-licensed-bridges.md)).

**Exit criteria**

- Workflow [W15](product/workflows.md#w15-field-inspection-on-a-phone-without-a-connection) passes.
- Workflow [W16](product/workflows.md#w16-add-an-extension-from-the-curated-catalog) passes.
- Workflow [W17](product/workflows.md#w17-work-on-one-project-at-two-sites) passes.
- A device whose access was revoked loses its tokens at once and its offline store at the next
  contact.
- An extension never reads or writes anything its user could not, verified by a permission test
  suite over every host function and message-protocol operation.
- A sealed bid is unreadable to every principal, including administrators, until its due date.
