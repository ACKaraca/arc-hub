# Glossary

Terms as Arc Hub uses them, one line each.

| Term | Meaning |
|---|---|
| Access level | One of Read, Download, Comment, Upload, Edit, Manage or Submit on a folder. |
| Affected record | A document or item whose revision and state a change order controls. |
| Annotation | A drawing on a media frame, stored with a note. |
| Arc exchange package | An open, documented container that connectors and the Hub use to carry geometry, semantics and identifiers of data from other applications. |
| Audit record | An append-only, hash-chained entry describing a security- or content-relevant action. |
| Bid package | A scope, bid form, invited companies and due date for collecting comparable bids. |
| Blob | File content stored once under its SHA-256 digest. |
| Booking | An allocation of a person or department to a project for a period. |
| Borrow | A user's exclusive right to modify one element of a shared document. |
| Bridge worker | A job worker whose operator has installed and licensed a conversion library that runs as a separate process. |
| Catalog (extension) | The extension versions an administrator has approved for installation. |
| Category | A classification that selects lifecycle, revision scheme, numbering scheme and property set. |
| Change order | A routed, approved change that releases its affected records together. |
| Change request | A proposed change evaluated before it becomes a change order. |
| Change set | An ordered batch of element operations from one client's operation journal. |
| Check-in / check-out | Committing a new version and releasing or keeping the lock / taking the lock and a working copy. |
| Clash | A detected geometric conflict identified by clash test and element pair. |
| Clash group | An aggregation of clashes reviewed and assigned together. |
| Company | An organization-level record of a participating firm; every member belongs to one. |
| Conflict copy / conflict document | Preserved work that could not become the latest version. |
| Conflict record | A change set that could not be applied, kept with both states until resolved. |
| Connector | An add-in that runs inside another application through its public API and exchanges data with Arc. |
| Consumption | A team's adoption of a package version as reference content. |
| Container state | Work in progress, shared, published or archived; the ISO 19650 state of a version. |
| Cross-organization share | A capped folder link between projects of different organizations. |
| Custom action | An administrator-defined menu action that opens a URL or starts a job. |
| Data stream | A source of timestamped readings linked to model elements. |
| Delivery plan | The list of expected deliverables with responsible company, milestone and target state. |
| Derivative | A product computed from a version: viewable, thumbnail, text, property index, references or conversion. |
| Design package | A team's curated definition of documents and element filters shared with other teams. |
| Document | A stable lineage of versions with a name and folder. |
| Document package | A named, versioned collection of document versions for submission. |
| Document set | An immutable, named list of document versions or sheet revisions. |
| Effective level | The access level a principal has after grants, inheritance and state caps. |
| Element index | The per-version store of model elements with identifiers, classes and typed properties. |
| Entity | A tracked production record such as a shot, asset or custom type. |
| Event | A CloudEvents record of a committed change with a per-organization sequence number. |
| Exchange | A named, versioned subset of elements of a source model offered to other applications. |
| Extension | A sandboxed WebAssembly component or isolated frame that adds pages or panels, with a declared permission set. |
| Finding | A result of a rule-based model check referencing elements. |
| Gate review | A review whose approval is required for a state transition. |
| Grant | A principal, a scope and an access level or set of record rights. |
| Guard plug-in | A sandboxed WebAssembly module that allows or refuses an operation. |
| Headless runner | A sandboxed process running an Arc application's command-line tool for a job. |
| Item | A master record of a part, assembly, document or purchased component. |
| Job definition | Runner, command or script, parameter schema, inputs, outputs and limits of an automation. |
| Legal hold | A protection that prevents permanent deletion of everything it covers. |
| Lifecycle | States, transitions, conditions, actions and caps that control release. |
| Lock | The exclusive right to add versions to a document. |
| Markup set | A named group of markups on a version. |
| Mobile device | A registered installation of a native mobile client, bound to one user. |
| Model set | A named set of model documents with transforms and discipline groups. |
| Module | Documents, Design collaboration, Product data, Production tracking or Construction records, enabled per project. |
| Offline set | The folders, documents, sheets, views and records a user keeps on a mobile device. |
| Operation | A typed, permission-checked server action; the only way domain state changes. |
| Organization | A tenant of the server that owns projects, companies and definitions. |
| Outbox | The ordered queue of changes made on a device and not yet accepted by the server. |
| Package version | A sealed, immutable result of a design package at the moment of sharing. |
| Peer | One server of a replication group, with a stable site identifier. |
| Pipeline step | A stage of production work such as modeling or lighting. |
| Playlist | An ordered list of submissions for review. |
| Principal | A user, group, company, project role or service account that can hold grants. |
| Property definition | A typed, validated field shared by documents, items, records and entities. |
| Published output | A document version registered with production context and upstream links. |
| Purchase order | A record that orders goods or services from a vendor, with lines linked to cost lines. |
| Record | An instance of a record type: issue, review, transmittal, change order, RFI, submittal, form, task and others. |
| Record type | The numbering, fields, workflow and permissions of a kind of record. |
| Reference | A link from a version to another document, pinned to a version or to latest. |
| Replication group | The servers that replicate a set of projects between them asynchronously. |
| Revision | A user-meaningful label spanning one or more versions, assigned by lifecycle or state rules. |
| Revision scheme | The sequence rules that produce revision labels and revision codes. |
| Role (project) | A named function such as document controller, used for assignment and grants. |
| Role (server) | All, web, worker or cache; what an `arc-hub` process performs. |
| Scene inventory | The list of Hub-sourced items loaded into the open scene of another application. |
| Service account | A non-human principal with grants and key-based credentials. |
| Shared document | An Arc native document in multi-user mode, stored as snapshot plus change sets. |
| Shared timeline | The project history of package shares, consumptions, updates and reverts. |
| Sheet | A numbered drawing extracted from a version and stacked by sheet number. |
| State cap | The maximum access level a container or lifecycle state allows a principal. |
| Step route | Ordered or parallel steps with participants and completion rules in a workflow. |
| Submission | A reviewable output of a task with media and status. |
| Suitability code | The status code that states what a shared or published container may be used for. |
| Task team area | A work-in-progress folder accessible only to one task team and information managers. |
| Team | A group of project members by discipline or company with a team area. |
| Transmittal | An immutable record of versions formally issued to recipients. |
| Version | An immutable, numbered state of a document. |
| Viewable | A streaming derivative loaded by the browser viewer. |
| Viewpoint | Camera, sections, visibility, selection and snapshot, as defined by BCF. |
| Working folder | The local root that mirrors Hub folder paths so references resolve. |
