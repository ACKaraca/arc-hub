# Assistant and agent access

Optional assistance and controlled access for AI agents. No Hub feature requires an assistant
service; content reaches a model provider only through an endpoint an administrator configured
and only for the content types they allowed. The suite rules for assistance levels, grounding, data
handling, provenance and evaluation are in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md).

Domain objects:

- **Assistant provider**: an administrator-configured model endpoint, including local models.
- **Agent client**: an OAuth client or service account used by an agent, with its scopes.
- **Assistant draft**: a proposed change or text that a user reviews before it is applied.
- **Assistance budget**: limits on requests, size and money per organization, project or user.
- **Digest**: a periodic summary of project activity for one user, built only from records that
  user can read.

## Configuration and access

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-001 | Provider configuration | Server or organization administrators configure one or more compatible model endpoints (hosted or local), models and limits. Assistance is disabled by default. | T3 | 3 |
| HB-ASSIST-002 | Content policy | Per organization, administrators choose which modules and content types may be sent to a provider and can redact personal fields. Each request records in the audit trail what was sent, when and to which endpoint. | T2 | 3 |
| HB-ASSIST-003 | Model Context Protocol endpoint | The server exposes a Model Context Protocol endpoint whose tools are derived from the REST API operations (search, read metadata, list and draft records, start jobs). Agents authenticate as a user through OAuth, and every tool call is permission-checked like an API call. | T2 | 4 |
| HB-ASSIST-004 | Agent boundaries | Write tools create drafts that a person applies. Agents cannot change permissions, delete permanently, decide reviews, sign or release; every agent call is audited with the agent client and rate-limited separately. | T2 | 4 |

## Assistance

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-005 | Project questions | Users ask questions about project records they can read (for example overdue reviews per company); the assistant answers by calling API tools and cites links to the records used. | T3 | 3 |
| HB-ASSIST-006 | Thread summaries | The assistant summarizes issue threads, review comments, RFI responses and change orders as a draft comment the user edits before posting. | T3 | 5 |
| HB-ASSIST-007 | Sheet metadata suggestions | When title-block regions do not yield values, the assistant proposes sheet number, title and revision from the page text for the uploader to confirm. | T3 | 5 |
| HB-ASSIST-008 | Name and property suggestions | The assistant proposes compliant names and property values from file content during upload; nothing is applied without confirmation. | T3 | 5 |
| HB-ASSIST-009 | Photo and issue tagging | The assistant proposes tags, locations and issue types for photos and new issues, shown as suggestions. | T3 | 5 |
| HB-ASSIST-010 | Submittal register extraction | From a specification document the assistant proposes submittal register items with section references for review before import. | T3 | 5 |

## Permissions, budgets and audit

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-011 | Permission-scoped retrieval | Every assistant answer, summary and search is built only from records the asking user can read at the time of the request: retrieval runs with that user's permissions, indexes and caches are never shared across permission scopes, and records cited in an answer are checked again before the answer is shown. | T2 | 3 |
| HB-ASSIST-012 | Assistance budgets | Administrators set budgets per organization, project and user for requests, request size and money at the prices they record for each provider; a request that would exceed a budget is refused before anything is sent, and users see their remaining budget. | T2 | 3 |
| HB-ASSIST-013 | Assistance audit report | Administrators see and export assistance activity by organization, project, user and agent client: requests, provider and model, content types sent, tools called, drafts created and applied, refusals and cost. Content is not stored in the report unless the organization's content policy enables it. | T2 | 3 |

## Coordination support

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-014 | Activity digests | Users subscribe to a daily or weekly digest of the projects they choose: new versions, issues assigned or due, reviews waiting, comments that mention them; every item links to its record, and the digest says which records it could not include. | T3 | 3 |
| HB-ASSIST-015 | Version change summaries | When a new version of a model or drawing is uploaded, a headless comparison job (HB-JOB-004) computes the differences and the assistant drafts a change description for the version note or transmittal, each statement citing the compared elements; the uploader edits it before publishing. | T3 | 3 |
| HB-ASSIST-016 | Duplicate issue detection | While a user creates an issue, the assistant lists existing open issues with similar text, location or elements, with the reason for each match, so the user can link instead of duplicating. | T3 | 3 |
| HB-ASSIST-017 | Review routing suggestions | When a review starts (HB-REVIEW-005), the assistant proposes a workflow template and reviewers from the document's type, discipline, project roles and past reviews of similar documents, with the reason for each suggestion; the initiator confirms or changes them. | T3 | 4 |
| HB-ASSIST-018 | Cross-project questions | Users ask questions across the projects they can read ("which projects use this wall type", "open issues about fire stopping on all sites"); the assistant answers from search, metadata and model data queries (HB-SEARCH-007) and cites each record and project. | T3 | 4 |

## Agent runs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-019 | Budgeted agent jobs | A user starts an assistant task over a declared set of project documents as a job (HB-JOB-006) with a budget; it runs headless Arc applications in the job sandbox (HB-JOB-005) and writes its results as new draft versions or draft records in a review state, never as released content, with a run record of every command and cost (HB-JOB-008). | T3 | 5 |
| HB-ASSIST-020 | Hand-off notes | A user attaches an assistant session's hand-off note (requests, applied proposal summaries, cited elements) to an issue, package or version; an Arc application that opens the record shows the note as context for its own assistant, and nothing in the note runs by itself. | T3 | 4 |


## Readiness

Readiness classes are defined in
[assistance](https://github.com/ACKaraca/arc-suite/blob/main/docs/architecture/assistance.md#readiness-classes).

| Class | ID | Checked by | Depends on |
|---|---|---|---|
| R1 | HB-ASSIST-005 | Permission check of cited records | HB-ASSIST-001, HB-ASSIST-011 |
| R1 | HB-ASSIST-006 | User edit before posting | HB-ISSUE-001 |
| R1 | HB-ASSIST-007 | Uploader confirmation | Sheet metadata extraction |
| R1 | HB-ASSIST-008 | Naming rules of the project | Information management naming rules |
| R1 | HB-ASSIST-009 | User confirmation | Image input in the provider |
| R1 | HB-ASSIST-010 | Section references | Specification text extraction |
| R1 | HB-ASSIST-011 | Permission re-check | HB-SEARCH-002 |
| R1 | HB-ASSIST-012 | Budget ledger | HB-ASSIST-001 |
| R1 | HB-ASSIST-013 | Audit trail | HB-ASSIST-002 |
| R1 | HB-ASSIST-014 | Permission-scoped retrieval | HB-ASSIST-011 |
| R1 | HB-ASSIST-015 | Headless comparison job | HB-JOB-004 |
| R1 | HB-ASSIST-016 | Issue queries | HB-ISSUE-001, HB-ISSUE-004 |
| R1 | HB-ASSIST-017 | Review template rules | HB-REVIEW-001, HB-REVIEW-005 |
| R1 | HB-ASSIST-018 | Search and model data queries | HB-SEARCH-007 |
| R2 | HB-ASSIST-019 | Run record, review state | HB-JOB-005, HB-JOB-006, HB-JOB-008, assistance in the Arc applications |
| R1 | HB-ASSIST-020 | Note validation | Hand-off notes in the Arc applications |
