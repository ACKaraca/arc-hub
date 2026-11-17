# Administration, audit and reporting

Server and organization administration, the audit trail, reports, dashboards, data extracts,
retention and legal hold, and complete data export.

Domain objects:

- **Audit record**: an append-only, hash-chained record of a security- or content-relevant action.
- **Report** and **dashboard**: predefined and composed views over records and audit data.
- **Data extract**: a scheduled export of project data in a documented schema.
- **Retention policy** and **legal hold**: rules for keeping, purging and protecting data.
- **Quota**: a per-organization limit on storage, projects or job time.

## Audit

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ADMIN-001 | Audit trail | The server appends records for sign-in and failed sign-in, permission changes, uploads, downloads, deletions, state transitions, review decisions, administrative actions, token and key creation and job runs. Each record holds time, actor (user, service account or job, with the user acted for), network address, client, object and a before and after summary. | T1 | 2 |
| HB-ADMIN-002 | Tamper evidence | Audit records are hash-chained per organization, and a daily digest of the chain head is stored separately. The administration command verifies a chain and reports the first break. | T2 | 2 |
| HB-ADMIN-003 | Audit search and export | Administrators filter audit records by actor, object, action and time, export them as CSV or JSON Lines, and stream them continuously to a syslog or HTTPS collector. | T1 | 2 |
| HB-ADMIN-004 | Audit retention | Audit records are kept for a configurable period, seven years by default, and never purged while a legal hold covers them. | T1 | 2 |

## Reports and extracts

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ADMIN-005 | Built-in reports | Reports cover members and permissions, documents per state, issues, reviews, transmittals, storage and activity per user, with filters and export as PDF, CSV and XLSX. | T1 | 2 |
| HB-ADMIN-006 | Scheduled reports | Any report can be scheduled to be emailed to members periodically as a link to its latest result. | T2 | 2 |
| HB-ADMIN-007 | Dashboards | Project and portfolio dashboards are composed from widgets (counts, charts over time, lists) based on saved queries and filtered by the viewer's permissions; each role can have a default dashboard. | T2 | 3 |
| HB-ADMIN-008 | Data extracts | Scheduled full or incremental extracts of records, version metadata, element queries and audit data are written as CSV or Parquet with a documented schema to a storage folder or offered for download, for business intelligence tools. | T2 | 3 |
| HB-ADMIN-009 | Usage analytics | Administrators see active users per day, most used folders and job activity; this data stays on the server and nothing is sent elsewhere. | T2 | 3 |

## Governance

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ADMIN-010 | Retention policies | Organizations and projects set retention per object type: keep versions for a period, purge trash after a period, archive projects inactive for a period. A preview lists what a policy would affect before it is enabled; execution runs as a job and is audited. | T2 | 3 |
| HB-ADMIN-011 | Legal hold | A hold on a project, folder or user prevents permanent deletion and purge of everything it covers, including trash and audit records, until it is released; active holds are listed with their reason. | T2 | 3 |
| HB-ADMIN-012 | Complete organization export | An organization exports all metadata in a documented JSON schema together with blobs, histories, records, markups, reviews and audit data, and the export imports into another Arc Hub server with identifiers preserved. | T1 | 2 |
| HB-ADMIN-013 | Quotas | Server administrators set per-organization quotas for storage, projects and daily job time. Users are warned at 80 percent; at 100 percent uploads and new jobs are refused while reading and downloading continue. | T2 | 2 |

## Administration

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ADMIN-014 | Server administration pages | Server administrators manage organizations, storage locations, default identity providers, email delivery, workers and health in the web client. | T1 | 2 |
| HB-ADMIN-015 | Organization self-service limits | Server administrators set whether users may create organizations and the default quotas applied to new organizations. | T2 | 2 |
| HB-ADMIN-016 | Administration command | The `arc-hub-admin` command migrates the database, creates organizations and administrators, backs up and restores, verifies audit chains and blob integrity, rotates keys, rebuilds indexes, and exports and imports organizations. | T1 | 2 |
| HB-ADMIN-017 | Localization | The web client, emails and reports are complete in English and Turkish; organizations set a default locale and users override it, with dates and numbers formatted by locale. | T1 | 2 |
| HB-ADMIN-018 | Branding | Server administrators set a logo, accent color, sign-in message and a link to terms of use. | T3 | 3 |
