# Assistant and agent access

Optional assistance and controlled access for AI agents. No Hub feature requires an assistant
service; content reaches a model provider only through an endpoint an administrator configured
and only for the content types they allowed.

Domain objects:

- **Assistant provider**: an administrator-configured model endpoint, including local models.
- **Agent client**: an OAuth client or service account used by an agent, with its scopes.
- **Assistant draft**: a proposed change or text that a user reviews before it is applied.

## Configuration and access

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-001 | Provider configuration | Server or organization administrators configure one or more compatible model endpoints (hosted or local), models and limits. Assistance is disabled by default. | T3 | 5 |
| HB-ASSIST-002 | Content policy | Per organization, administrators choose which modules and content types may be sent to a provider and can redact personal fields. Each request records in the audit trail what was sent, when and to which endpoint. | T2 | 5 |
| HB-ASSIST-003 | Model Context Protocol endpoint | The server exposes a Model Context Protocol endpoint whose tools are derived from the REST API operations (search, read metadata, list and draft records, start jobs). Agents authenticate as a user through OAuth, and every tool call is permission-checked like an API call. | T2 | 4 |
| HB-ASSIST-004 | Agent boundaries | Write tools create drafts that a person applies. Agents cannot change permissions, delete permanently, decide reviews, sign or release; every agent call is audited with the agent client and rate-limited separately. | T2 | 4 |

## Assistance

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-ASSIST-005 | Project questions | Users ask questions about project records they can read (for example overdue reviews per company); the assistant answers by calling API tools and cites links to the records used. | T3 | 5 |
| HB-ASSIST-006 | Thread summaries | The assistant summarizes issue threads, review comments, RFI responses and change orders as a draft comment the user edits before posting. | T3 | 5 |
| HB-ASSIST-007 | Sheet metadata suggestions | When title-block regions do not yield values, the assistant proposes sheet number, title and revision from the page text for the uploader to confirm. | T3 | 5 |
| HB-ASSIST-008 | Name and property suggestions | The assistant proposes compliant names and property values from file content during upload; nothing is applied without confirmation. | T3 | 5 |
| HB-ASSIST-009 | Photo and issue tagging | The assistant proposes tags, locations and issue types for photos and new issues, shown as suggestions. | T3 | 5 |
| HB-ASSIST-010 | Submittal register extraction | From a specification document the assistant proposes submittal register items with section references for review before import. | T3 | 5 |
