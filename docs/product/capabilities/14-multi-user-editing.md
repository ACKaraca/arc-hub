# Multi-user editing

Element-level multi-user editing of Arc native documents, and branches and merges of model data.
Change sets come from the operation journal of each Arc document; there is no CRDT, and a
conflict never discards work.

Domain objects:

- **Shared document**: an Arc native document in multi-user mode, stored as a snapshot plus change sets.
- **Change set**: an ordered batch of element operations from one client, with base sequence number.
- **Borrow**: a user's exclusive right to modify one element of a shared document.
- **Conflict record**: a change that could not be applied, preserved with both states.
- **Branch**: a separate line of versions or change sets of one model document.
- **Merge**: the application of a branch's changes to a target line.

## Sessions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SESSION-001 | Enable multi-user mode | A project administrator switches an Arc native document to multi-user mode when its application declares support. The server stores the current snapshot and accepts change sets from then on; the document's version history continues. | T3 | 5 |
| HB-SESSION-002 | Join a session | An Arc application opening a shared document downloads the latest snapshot and the change sets after it, applies them and shows the active participants. | T3 | 5 |
| HB-SESSION-003 | Change sets | The application uploads change sets taken from its operation journal on synchronize or automatically at an interval. The server assigns the next sequence number and rejects a change set that modifies an element borrowed by someone else. | T3 | 5 |
| HB-SESSION-004 | Element borrowing | Before modifying elements, the application requests borrows for the element set it computes (including dependent elements). The server grants only if no one else holds them, with a target response time of 200 ms on a local network; borrows are released on synchronize or relinquish. | T3 | 5 |
| HB-SESSION-005 | Borrow requests | A user asks for an element borrowed by someone else; the holder is notified and grants or declines. An administrator can force release; the holder's unsynchronized changes to those elements are then kept as a conflict record. | T3 | 5 |
| HB-SESSION-006 | Presence | Participants see who is in the session, which elements each holds and, if shared, where each participant's view is. | T3 | 5 |
| HB-SESSION-007 | Receive changes | Applications receive new change sets by push or at an interval and apply them in sequence order as transactions, followed by regeneration. | T3 | 5 |
| HB-SESSION-008 | Conflict preservation | A change set that cannot be applied, for example because an element was deleted remotely, is stored as a conflict record with both states; the user resolves it in the application by choosing or combining states. | T3 | 5 |
| HB-SESSION-009 | Offline work | Offline edits are allowed on elements borrowed before disconnecting. Edits to other elements are held as pending changes that request borrows on reconnect; refused ones become conflict records. | T3 | 5 |
| HB-SESSION-010 | Snapshots | A job periodically builds a new snapshot by applying change sets with the headless application; earlier change sets remain available for history according to retention. | T3 | 5 |
| HB-SESSION-011 | Publish a version | The state of a shared document at a sequence number is published as a regular document version for reviews, transmittals and packages. | T3 | 5 |
| HB-SESSION-012 | Change history | Each change set lists author, time, commands and elements; users filter history by element and revert a change set, which adds its inverse as a new change set. | T3 | 5 |
| HB-SESSION-013 | Session health | Administrators see snapshot age, change-set backlog, active borrows and storage size per shared document, with warnings when thresholds are exceeded. | T3 | 5 |

## Branches and merges

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SESSION-014 | Branches | Users create a named branch of a model document from any version; the branch has its own versions, and the main line is unaffected. | T3 | 3 |
| HB-SESSION-015 | Merge | Merging a branch computes an element-level three-way comparison from the journals of the common base, the branch and the target. A job applies non-conflicting operations with the headless application; conflicts are listed for resolution in the application; the result is a new version on the target with a merge record. | T3 | 5 |
| HB-SESSION-016 | Merge requests | A merge can require a merge request with element comparison (HB-VIEW-023, HB-VIEW-024) and an approval workflow before it is applied. | T3 | 5 |

## Browser participants

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SESSION-017 | Browser participants | The browser build of an Arc product (HB-FILE-028) joins a shared document like a desktop application: it downloads the snapshot and change sets, borrows elements, uploads change sets and shows presence, with the same conflict preservation (HB-SESSION-008). | T3 | 5 |
