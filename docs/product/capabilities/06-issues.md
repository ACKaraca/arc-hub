# Issues

Tracked problems, questions and actions tied to locations in drawings and models. Issues are BCF
topics by design, so they travel unchanged between Arc Hub, Arc applications and other BCF-capable
tools.

Domain objects:

- **Issue**: a numbered record with type, status, assignee, dates, references and a BCF GUID.
- **Issue type**: a configurable kind of issue with its own workflow, fields and permissions.
- **Viewpoint**: camera, sections, visibility, selection and snapshot (the BCF viewpoint model).
- **Comment**: a thread entry with optional attachment and viewpoint.
- **Root cause** and **location**: project lists used to classify issues.

## Issue records

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ISSUE-001 | Issue record | An issue has an immutable project number with a type prefix, title, description, type, status, priority, assignee (user, role or company), watchers, start and due dates, location, labels, root cause, custom properties, BCF GUID, author and timestamps. | T1 | 2 |
| HB-ISSUE-002 | Issue types | Project administrators define a two-level classification of categories and types. Each type has its own status workflow, custom fields, default assignee and watchers, number prefix and permissions (HB-ACCESS-021). | T1 | 2 |
| HB-ISSUE-003 | Status workflows | Each type defines statuses, each mapped to one of the categories draft, open, in progress, resolved, closed or void, and the allowed transitions with the principals allowed to make them and fields required on transition (for example a root cause to close). The default workflow is Draft, Open, In progress, Resolved, Closed, plus Void. | T1 | 2 |
| HB-ISSUE-004 | Pins and viewpoints | An issue is placed on a sheet position or a 3D point or element, with a viewpoint holding camera, sections, visibility, selected element identifiers and a snapshot image. Opening the issue restores the viewpoint in the browser or in an Arc application. | T1 | 2 |
| HB-ISSUE-005 | References | Issues link to document versions, model elements by identifier, other issues, reviews, construction records, change orders and clash groups. A reference to a version shows when newer versions of that document exist. | T1 | 2 |
| HB-ISSUE-006 | Comments and attachments | Issues have threaded comments with a limited Markdown subset, mentions and attachments uploaded or referenced from project documents. Edited comments keep their history; a comment can carry its own viewpoint. | T1 | 2 |
| HB-ISSUE-007 | Reminders | Assignees and watchers receive reminders at a configurable offset before the due date and when an issue becomes overdue; overdue issues are flagged in every list. | T1 | 2 |
| HB-ISSUE-008 | Bulk edit | Users update assignee, due date, labels and status of many issues at once. Only valid transitions are applied, and the result reports each issue that could not be changed with the reason. | T1 | 2 |
| HB-ISSUE-009 | Issue lists | Issue lists filter on every field including custom properties, sort, group, choose columns and show counts. Saved views are personal or shared with the project. | T1 | 2 |
| HB-ISSUE-010 | Issue history | Every field change, comment and transition is recorded with author and time and shown on the issue. | T1 | 2 |
| HB-ISSUE-011 | Issue templates | Per type, administrators define prefilled issue templates for recurring issues; creating from a template copies its values. | T2 | 2 |
| HB-ISSUE-012 | Root causes | A project list of root causes is selectable on issues and can be required when closing; reports group by root cause. | T2 | 2 |
| HB-ISSUE-013 | Locations | Projects keep a location tree (for example building, level, zone, room) imported from CSV or from the spatial structure of a model version; issues reference a location node and filter by subtree. | T2 | 3 |
| HB-ISSUE-014 | Draft issues | Draft issues are visible only to their author and type administrators until moved to an open status. | T2 | 2 |
| HB-ISSUE-015 | Company-scoped visibility | An issue type can restrict members to issues created by or assigned to their own company, while administrators and selected roles see all. | T2 | 2 |
| HB-ISSUE-016 | Issue metrics | Each project shows open and closed issues per period, average time to close, overdue issues per assignee company and ageing buckets, exportable as CSV. | T2 | 2 |
| HB-ISSUE-017 | Duplicate suggestions | While an issue is being created, the server suggests open issues with similar titles at the same location or on the same elements. | T3 | 3 |
| HB-ISSUE-018 | Replies by email | Replying to an issue notification email adds a comment to the issue as the replying member, matched by a signed reply address; replies from unknown senders are rejected. | T3 | 4 |

## Exchange

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ISSUE-019 | Issue export | Issue lists export as CSV, XLSX, a PDF report with snapshots and comments, and BCF 2.1 or 3.0 archives. | T1 | 2 |
| HB-ISSUE-020 | BCF import | Importing a BCF archive creates or updates issues matched by topic GUID. When both sides changed a field, the server keeps its value, appends the imported values as a comment and lists the conflict in the import report. Authors map by email. | T1 | 2 |
| HB-ISSUE-021 | BCF API | The server implements the buildingSMART BCF API (version 3.0) for projects, topics, comments, viewpoints, document references and project extensions, so BCF-capable tools synchronize issues directly. | T1 | 2 |
| HB-ISSUE-022 | Issues in Arc applications | Arc desktop applications list, create and update issues with viewpoints through the Hub API; selecting an issue restores its viewpoint and element selection in the open model. | T1 | 2 |
