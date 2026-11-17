# Integrations

Connections between Arc Hub and Arc applications, collaboration tools, office software, enterprise
systems and earlier data stores. Integrations use the public API, webhooks and jobs; none has
privileged access.

Domain objects:

- **Integration registration**: the clients, webhooks, service accounts and publishers of one integration.
- **Import manifest**: a documented description of documents, versions and metadata to import.
- **Calendar feed**: a secret-URL iCalendar feed of a user's dates.

## Arc suite

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INTEG-001 | Arc application integration | Every Arc desktop application uses the Hub through the `arc-hub-api` client for sign-in, browsing, opening, saving versions, locks, issues, reviews, packages and jobs, with the same behavior across applications (HB-SYNC-001 to HB-SYNC-007). | T1 | 2 |
| HB-INTEG-002 | Integration registry | Organization administrators see every integration (OAuth clients, webhooks, service accounts, publishers, extensions) with owner, scopes and last activity, and disable all parts of an integration in one action. | T2 | 2 |

## Collaboration tools

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INTEG-003 | Chat notifications | Selected events are posted to chat systems through their incoming webhook URLs using configurable message templates. | T2 | 3 |
| HB-INTEG-004 | Calendar feeds | Each user has an iCalendar feed of due dates, review steps and milestones at a secret URL that can be regenerated. | T2 | 3 |
| HB-INTEG-005 | Email ingestion | Each project can have an inbound address: attachments of received emails from members are filed into an intake folder with the email as a record, and mail from unknown senders is rejected. | T3 | 4 |
| HB-INTEG-006 | Browser editing of office documents | The server implements the WOPI host interface, so a separately deployed office suite server edits Office documents in the browser; saving creates a new version. | T3 | 4 |
| HB-INTEG-007 | Map context | Project locations and geolocated photos display on maps from a configured tile server; without one, no map requests are made. | T3 | 3 |

## Data and systems

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INTEG-008 | Business intelligence | BI tools read data extracts (HB-ADMIN-008) and the query API; no proprietary connector is required. | T2 | 3 |
| HB-INTEG-009 | Folder import | A one-time import from a local folder, WebDAV share or S3 bucket preserves the tree and file modification times as version times and reports naming compliance. | T2 | 2 |
| HB-INTEG-010 | Migration import | A documented manifest format (CSV or JSON plus files) imports documents with ordered versions, properties, states and authors; issues import through BCF and production entities through CSV, so data from other systems arrives with its history. | T2 | 3 |
| HB-INTEG-011 | Enterprise systems | Product data publishes to and receives from enterprise systems through release publication (HB-ECO-012) and inbound master data (HB-ECO-013); reference scripts for common file layouts run as jobs. | T2 | 3 |

## Extensions

Extensions add pages, panels and computations to the web client without running third-party code
in the Hub's own pages. They are sandboxed WebAssembly components or isolated frames that declare
a permission set, and administrators curate the catalog they come from. The decision is recorded in
[ADR-0008](../../adr/0008-sandboxed-extensions-and-curated-catalog.md). The catalog handles no payments.

Domain objects:

- **Extension**: a package with manifest, version, publisher and content digest.
- **Permission set**: the scopes, network origins, storage and placements an extension declares.
- **Catalog entry**: an extension version that an administrator has approved for installation.
- **Extension installation**: an enabled extension in an organization or project, with its settings.

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-INTEG-012 | Extension packages | An extension package contains a manifest (identifier, version, publisher, declared permission set, entry points, kind) and content addressed by digest. The kind is a WebAssembly component run by the server or an isolated frame run in the browser. | T3 | 5 |
| HB-INTEG-013 | Isolated frames | A frame extension renders in a sandboxed frame served from a separate origin, with a content security policy that allows only the origins it declared, no access to Hub cookies or storage, and a message protocol with the host that carries a short-lived token of the viewing user limited to the declared scopes. | T3 | 5 |
| HB-INTEG-014 | WebAssembly components | A component extension runs on the server in the component model without ambient authority, with CPU, memory and time limits, reads and writes only through host functions the permission set allows, and returns declarative views (tables, forms, charts, text) that the host renders, so no third-party script runs in the Hub's page. | T3 | 5 |
| HB-INTEG-015 | Permission sets | Administrators see the declared permission set before approving an extension and approve it as a whole. A new version that asks for more permissions needs a new approval. At run time the extension's rights are the intersection of its approved set and the permissions of the user it acts for; it never exceeds the user. | T3 | 5 |
| HB-INTEG-016 | Placements | Extensions place pages and panels on the project home, document and record pages, the viewer sidebar, record menus (HB-API-023) and administration pages, shown only to the principals the installation names. | T3 | 5 |
| HB-INTEG-017 | Extension catalog | Organization administrators, and server administrators for all organizations, curate a catalog: they add a package by upload or from a registry address they choose (an outbound call only when configured), review the manifest, approve a version, pin the version in use and withdraw entries. Users and project administrators enable only catalog entries. The catalog has no prices, purchases or ratings. | T3 | 5 |
| HB-INTEG-018 | Package integrity | Packages are signed by the publisher key recorded at approval and their digest is verified at install and load. Unsigned packages are accepted only in developer mode (HB-INTEG-021). An update shows the permission difference and the changed digest before approval. | T3 | 5 |
| HB-INTEG-019 | Extension storage and settings | Each installation gets key-value storage scoped to organization, project or user with a quota, and a settings form generated from a schema in the manifest. | T3 | 5 |
| HB-INTEG-020 | Lifecycle, audit and kill switch | Install, enable, disable, update and removal are audited; calls an extension makes are attributed to the extension acting for the user. The integration registry (HB-INTEG-002) lists extensions and disables one at once, and a faulting extension is isolated without affecting the page or other extensions. | T3 | 5 |
| HB-INTEG-021 | Developer mode | An administrator enables developer mode for a test project, where unsigned extensions load from a local address with a development token, and a validator checks a manifest against the schema. Developer mode is off by default and cannot be enabled for projects holding released data. | T3 | 5 |
