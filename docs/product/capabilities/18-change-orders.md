# Product data: change management and enterprise integration

Change requests and change orders that control the release of documents and items, product data
reports, and the publication of released data to enterprise systems.

Domain objects:

- **Change request**: a proposed change with problem description and affected records.
- **Change order**: an approved-path change with type, workflow, affected records and outcome.
- **Change type**: workflow, numbering, required fields and approver rules of a kind of change.
- **Affected record**: a document or item with its current and target revision and state.
- **Publication run**: one delivery of released data to an enterprise system, with its acknowledgments.

## Change requests and orders

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ECO-001 | Change requests | A change request records problem description, requester, affected items and documents, priority and attachments, runs through an evaluation workflow and can be promoted into one or more change orders. | T1 | 3 |
| HB-ECO-002 | Change orders | A change order has number, type, title, reason code, affected records with target revision and state, attachments, markups and related change orders, and is routed through steps with approvers, completion rules and due dates on the shared workflow engine. | T1 | 3 |
| HB-ECO-003 | Change types | Administrators define change types with workflow, numbering scheme, required fields and approver rules, for example approvers required by the category of the affected items. | T1 | 3 |
| HB-ECO-004 | Reservation of affected records | A record added to an active change order is reserved: no other change order can include it, and its lifecycle transitions happen only through that change order. | T1 | 3 |
| HB-ECO-005 | Before and after | Each affected record shows its current released revision beside its working revision, with document comparison and BOM comparison (HB-ITEM-012). | T1 | 3 |
| HB-ECO-006 | Approvals | Approvers decide with a comment; steps can require a minimum number of approvers, signatures (HB-REVIEW-016) and allow delegation. | T1 | 3 |
| HB-ECO-007 | Release on approval | Closing an approved change order transitions all affected records to their target states with revision changes as one atomic operation. If any record fails a condition, no record changes and every failure is listed. | T1 | 3 |
| HB-ECO-008 | Change order markups | Markups made on affected versions inside a change order belong to it and are visible to its participants. | T2 | 3 |
| HB-ECO-009 | Reminders and escalation | Change order steps send reminders and escalate overdue steps as reviews do (HB-REVIEW-011). | T2 | 3 |
| HB-ECO-010 | Change reporting | Reports show open change orders by type and age, cycle time per step and records released per period, exportable as CSV and PDF. | T2 | 3 |

## Product data reports

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ECO-011 | Product data reports | Built-in reports cover lifecycle state summary, revision history, checked-out documents, item master list and multi-level BOM, exportable as PDF and CSV; additional report templates run as jobs over the API. | T2 | 3 |

## Enterprise systems

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ECO-012 | Release publication | On release events the server publishes item master data and structures, in full or as a delta, through a configured publisher: a file drop (CSV, JSON or XML into a folder or bucket), an HTTPS call with retries, or a job script. Each item revision records sent, accepted or rejected with the returned message. | T2 | 3 |
| HB-ECO-013 | Inbound master data | Enterprise systems update item properties such as cost, supplier and stock status through the API. Properties marked as owned by an external system are read-only in the Hub. | T2 | 4 |
| HB-ECO-014 | Publication log | Every publication run records payload digest, status, retries and errors and can be replayed. | T2 | 3 |
| HB-ECO-015 | Inbound change requests | External systems create change requests through the API, with the source system recorded on the request. | T3 | 4 |
