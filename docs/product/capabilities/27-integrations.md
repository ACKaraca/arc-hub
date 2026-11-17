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
| HB-INTEG-002 | Integration registry | Organization administrators see every integration (OAuth clients, webhooks, service accounts, publishers) with owner, scopes and last activity, and disable all parts of an integration in one action. | T2 | 2 |

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
