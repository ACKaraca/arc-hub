# Organizations and projects

Tenancy, project setup, templates, the company directory and the notification layer that every
module shares. Organizations are the isolation boundary of an Arc Hub server; projects are the
working unit that enables modules and holds content.

Domain objects:

- **Server**: one Arc Hub installation with its server administrators and storage locations.
- **Organization**: a tenant that owns projects, companies, groups, templates and definitions.
- **Project**: a container of folders, records and module settings inside one organization.
- **Project template**: a by-value snapshot of project configuration used to create projects.
- **Company**: an organization-level record of a participating firm.
- **Notification** and **activity event**: user-facing messages derived from domain events.

## Organizations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ORG-001 | Organization tenancy | An organization owns its projects, companies, groups, templates, property definitions and settings. No record of one organization is readable from another unless it is shared explicitly through a cross-organization share (HB-PKG-011). One server hosts any number of organizations. | T1 | 2 |
| HB-ORG-002 | Organization creation | A server administrator creates an organization with a display name, an immutable short code (2–16 characters, unique on the server), default locale, time zone and storage location. Self-service creation can be enabled per server, subject to the quota in HB-ADMIN-015. | T1 | 2 |
| HB-ORG-003 | Organization settings | Organization administrators set the display name, logo, default working calendar, default display units, allowed identity providers, session policy and email sender name. Every change is written to the audit trail with the previous value. | T1 | 2 |
| HB-ORG-004 | Storage location per organization | Each organization is bound to one blob storage location defined by the server administrator (a filesystem path or an S3-compatible bucket with region). All blobs and derivatives of the organization's projects are written there, which gives each tenant a defined data residency. | T2 | 2 |
| HB-ORG-005 | Usage summary | The server computes daily per-organization and per-project totals of stored bytes (originals, derivatives, trash), member counts and job minutes, viewable and exportable as CSV. The figures are informational; nothing is metered or charged per call. | T2 | 2 |
| HB-ORG-006 | Organization deactivation | Deactivating an organization makes all its projects read-only and blocks sign-in for its members except organization owners, who can export data (HB-ADMIN-012) or reactivate. Deactivation never deletes content. | T2 | 2 |

## Projects

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ORG-007 | Project creation | A permitted user creates a project with name, number (unique in the organization), short code used by naming standards, description, start and end dates, address, optional geolocation with coordinate reference system code, and a project kind (building and infrastructure, product, media, general). The kind preselects modules; the creator becomes project administrator. | T1 | 2 |
| HB-ORG-008 | Project modules | Each project enables any combination of the Documents, Design collaboration, Product data, Production tracking and Construction records modules. Each member has an access level per module: none, member or administrator. Disabling a module hides it from the web client and API and keeps its data. | T1 | 2 |
| HB-ORG-009 | Project properties | Organization administrators define project-level properties (text, number, date, list, user) that appear as columns and filters in the project list and as dimensions in reports. | T2 | 2 |
| HB-ORG-010 | Project list | Users see the projects they belong to, with search, filters on status, kind and project properties, sorting, favorites and recently opened projects. Organization administrators can list all projects. | T1 | 2 |
| HB-ORG-011 | Project status | A project is active, archived or a template. Archived projects are read-only, hidden from default lists, still searchable by administrators and restorable to active. Deleting a project requires the archived status and a typed confirmation and moves it into retention (HB-ADMIN-010). | T1 | 2 |
| HB-ORG-012 | Project administrators | A project always has at least one administrator; the last administrator cannot leave or be removed. Organization administrators can assign or replace project administrators of any project. | T1 | 2 |
| HB-ORG-013 | Project copy | Copying a project duplicates its configuration (modules, folder tree, folder settings, permissions, roles, workflows, record types, naming standard) and optionally its members and the latest versions of selected folders. Copied documents start a new version history whose first version records its origin. | T2 | 2 |

## Templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ORG-014 | Project templates | A template captures modules, folder tree with permissions and folder settings, roles, default members, naming standard, property definitions, issue types, review workflows, transmittal templates, lifecycles, numbering schemes, pipeline steps, status lists and automation rules. Creating a project from a template copies everything by value; later template edits never change existing projects. | T1 | 2 |
| HB-ORG-015 | Template from project | An administrator saves the configuration of an existing project as a new template; content, records and members are excluded unless explicitly selected. | T2 | 2 |
| HB-ORG-016 | Template exchange | Templates export as a documented JSON file and import into another organization or server. Import validates the file and reports unresolved references (users, groups, companies) that must be mapped before the template is saved. | T2 | 3 |
| HB-ORG-017 | Template drift | For a project created from a template, the server lists configuration differences between the project and the current template. An administrator applies selected template changes to the project as one audited operation. | T3 | 3 |

## Companies

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ORG-018 | Company directory | Each organization keeps companies with name, short code (usable as the originator field of a naming standard), trade or discipline, address, contact and external identifiers. Every organization member belongs to exactly one company of that organization. | T1 | 2 |
| HB-ORG-019 | Companies in projects | Companies are added to projects and can then be used as permission principals, issue and review assignees, transmittal recipients and report dimensions. | T1 | 2 |
| HB-ORG-020 | Company import | Companies import from CSV with a preview showing new, updated and rejected rows with reasons; probable duplicates (same short code or same name ignoring case) are flagged for merge. | T2 | 2 |

## Notifications and activity

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ORG-021 | In-app notifications | Each user has an inbox of notifications (assignment, mention, due soon, overdue, review requested, transmittal received, job failed) with read state and a link to the object. Notifications are retained for 90 days by default, configurable per organization. | T1 | 2 |
| HB-ORG-022 | Email notifications | Users choose per event category between immediate email, a daily digest at a chosen local time, or none. Emails contain links, never file attachments, and every email carries a one-click unsubscribe for its category. | T1 | 2 |
| HB-ORG-023 | Mentions | Mentioning a user or group in any comment notifies them if they can read the object. Mentioning someone without access warns the author and never grants access. | T1 | 2 |
| HB-ORG-024 | Activity feed | Each project has a chronological feed of events filtered by the reader's permissions, with filters by module, user and object type; every object also has its own history tab built from the same events. | T1 | 2 |
| HB-ORG-025 | Subscriptions | Users follow a folder, document, record or saved search and are notified of changes to it according to their notification preferences. | T2 | 2 |
| HB-ORG-026 | Project announcements | Project administrators publish an announcement shown as a banner to all members until it expires, optionally also sent by email. | T3 | 3 |
