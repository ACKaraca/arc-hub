# Product data: files, revisions and lifecycles

Check-in and check-out, versions and revisions, lifecycles with state-based security, categories,
property mapping and numbering for engineering data. Product data uses the same documents,
versions and locks as the Documents module and adds controlled release.

Domain objects:

- **Working folder**: the local root that mirrors Hub folder paths so references resolve.
- **Revision**: a user-meaningful label spanning one or more versions, assigned by lifecycle rules.
- **Revision scheme**: the sequence rules that produce revision labels.
- **Lifecycle definition**: states, transitions, conditions, actions and state caps.
- **Category**: a classification that selects lifecycle, revision scheme, numbering and property set.
- **Property mapping**: a rule linking a Hub property to a property inside a file format.
- **Numbering scheme**: segments that generate unique numbers for documents, items and change orders.

## Check-in and check-out

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PDM-001 | Check-out | Check-out takes the document lock (HB-FILE-019) and places the latest version in the working folder, recording holder, machine and time. In folders with product data enabled, editing requires check-out. | T1 | 3 |
| HB-PDM-002 | Check-in | Check-in uploads a new version with comment and optionally keeps the document checked out. Dependent documents checked in together (for example an assembly with modified parts) commit as one atomic change: all versions are created or none. | T1 | 3 |
| HB-PDM-003 | Undo check-out | Undo releases the lock, and the Arc application restores the server version in the working folder after confirmation. | T1 | 3 |
| HB-PDM-004 | Get latest or specific | Users download read-only copies at latest version, latest released revision or a chosen version, with options to include referenced children and referencing parents and drawings; locally modified files are never overwritten without confirmation. | T1 | 3 |
| HB-PDM-005 | Working folder mapping | Each project maps to a working folder root; relative paths mirror Hub folder paths so references resolve on every machine. Arc applications configure the mapping automatically. | T1 | 3 |
| HB-PDM-006 | Administrative undo check-out | Administrators release another user's check-out with a reason; the holder's local files are untouched and the holder is notified. | T1 | 3 |
| HB-PDM-007 | Offline reservation | Users check out before going offline and check in later. If the check-out was overridden meanwhile, the check-in is kept as a conflict document instead of becoming the latest version. | T2 | 3 |
| HB-PDM-008 | Generic file support | Files of other CAD applications receive versions, categories, lifecycles and numbering. References are extracted only from formats with published specifications; others are related manually as attachments. | T1 | 3 |

## Versions and revisions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PDM-009 | Revision schemes | Revision schemes have primary, secondary and tertiary segments; sequences are letters with configurable excluded letters, zero-padded numbers or custom lists. Categories select the scheme; container revision codes use the same engine (HB-INFO-016). | T1 | 3 |
| HB-PDM-010 | Revisions | Lifecycle transitions assign revision labels; one revision spans all versions until the next revision. Documents display both, for example revision B, version 14, and the revision history lists the versions of each revision. | T1 | 3 |
| HB-PDM-011 | Manual revision change | An authorized role sets the next primary or secondary revision outside a transition, with a mandatory reason recorded in history. | T2 | 3 |
| HB-PDM-012 | Latest released retrieval | Clients retrieve either the latest version or the latest released revision; references can be pinned to latest released. | T1 | 3 |

## Lifecycles

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PDM-013 | Lifecycle definitions | A lifecycle defines states with flags (released, locked against check-out, obsolete) and a state cap per principal, and transitions with allowed principals, conditions and actions. | T1 | 3 |
| HB-PDM-014 | Transition conditions | Conditions include required property values, all referenced children in released states, no active check-out, no open change order unless the transition comes from that change order, and guard plug-ins (HB-JOB-015). A refused transition lists every failed condition. | T1 | 3 |
| HB-PDM-015 | Transition actions | Actions bump the revision, set properties, queue jobs (for example PDF, STEP or viewable export by a headless application), write properties into the file, notify principals and release linked items. | T1 | 3 |
| HB-PDM-016 | State-based security | The state caps of lifecycle states restrict reading, adding versions, deleting and changing state per principal (HB-ACCESS-020), for example making released revisions read-only for designers. | T1 | 3 |
| HB-PDM-017 | Bulk state change | Users change the state of many documents at once, optionally including referenced children or referencing parents. A preview shows condition results per document, and the change applies to all documents or to none. | T1 | 3 |
| HB-PDM-018 | Lifecycle reassignment | When a document's category changes to one with another lifecycle, an administrator maps old states to new states and the change is recorded in history. | T2 | 3 |

## Categories, properties and numbering

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PDM-019 | Property mapping | Mapping rules link Hub properties to file properties per format: Arc document properties and DWG drawing properties and title-block attributes in both directions; PDF metadata, STEP header and Office document properties read-only. Writing back happens in the Arc application at check-in or by a job that creates a new version. | T1 | 3 |
| HB-PDM-020 | Categories | Documents, folders, items and change orders receive a category from ordered rules evaluated at first check-in (file extension, folder path, property value), or manually by an authorized role. The category selects lifecycle, revision scheme, numbering scheme and property set. | T1 | 3 |
| HB-PDM-021 | Property compliance | When a mapped Hub value differs from the value in the file, the document shows a mismatch indicator; users either update the file through a job or accept the file value. | T2 | 3 |
| HB-PDM-022 | Numbering schemes | Schemes combine fixed text, counters with padding and start value, list choices, property values, date parts and patterned free text, with counters kept per prefix. Arc applications reserve numbers through the API when creating files; reserved numbers are never reused, so gaps are allowed. | T1 | 3 |
| HB-PDM-023 | External number source | A numbering segment can request its value from an enterprise system through an outgoing call with a 5-second timeout; on failure the creation is refused with the error message. | T3 | 4 |
| HB-PDM-024 | Unique file names | Projects or organizations can require file names unique across all folders, enforced at check-in, so references by file name stay unambiguous. | T2 | 3 |
| HB-PDM-025 | Attachments | Documents, items and change orders carry non-hierarchical attachments to supporting documents such as specifications; attachments appear in where-used queries as their own relation type. | T1 | 3 |

## Release outputs and archives

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PDM-026 | Release outputs | Released revisions receive PDF and viewable outputs through transition actions, so readers view released data without the authoring application. | T1 | 3 |
| HB-PDM-027 | Project archive | A completed product project exports to a documented archive (blobs plus JSON metadata) and leaves active storage; restore returns it with identifiers, histories and revisions intact. | T2 | 4 |
