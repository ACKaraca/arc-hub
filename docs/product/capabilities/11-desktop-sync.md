# Desktop synchronization

Working with Hub content from the desktop: native integration in every Arc application, a sync
agent for other files, and WebDAV access. Desktop work never depends on a connection; changes are
reconciled without discarding anyone's work.

Domain objects:

- **Hub connection**: a server URL and signed-in account in an Arc application or the sync agent.
- **Local cache**: content-addressed copies of versions shared by Arc applications on one machine.
- **Sync root**: a local folder mirroring selected Hub folders.
- **Conflict copy**: a preserved local change that could not become the latest version.

## Arc applications

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SYNC-001 | Hub connection | Every Arc desktop application connects to one or more Hub servers (HB-ACCESS-008) and browses projects and folders in its open and save dialogs; the last listing is cached for offline browsing. | T1 | 2 |
| HB-SYNC-002 | Open from the Hub | Opening a document downloads the version into the local cache, addressed by digest and shared across Arc applications, and opens it. Referenced documents are downloaded at their referenced versions. The document is read-only unless the user holds its lock. | T1 | 2 |
| HB-SYNC-003 | Save to the Hub | Saving to the Hub creates a new version with comment; naming and required properties are validated before upload, and in programmatic paths they are command parameters, never dialogs. The upload runs in the background and resumes after interruptions. | T1 | 2 |
| HB-SYNC-004 | Edit and lock | Starting to edit acquires the document lock, automatically if the user chooses. Editing without the lock, for example offline, is allowed with a warning; if another version was added meanwhile, the upload is stored as a separate conflict document linked to the original and a comparison is offered. | T1 | 2 |
| HB-SYNC-005 | Status indicators | Arc applications show for each open or listed document whether it is the latest version, locked and by whom, or modified locally. | T1 | 2 |
| HB-SYNC-006 | Reference updates | When referenced documents have newer versions, the application lists them and the user updates selected references; the change is recorded in the next saved version. | T2 | 2 |
| HB-SYNC-007 | Upload of local derivatives | Arc applications upload viewables, sheet sets and exports they produced locally together with the version, so the server does not recompute them (HB-DATA-005). | T2 | 3 |

## Sync agent

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SYNC-008 | Sync agent | A background program for Windows, macOS and Linux mirrors selected projects and folders into a local sync root for applications that do not connect to the Hub themselves. | T2 | 3 |
| HB-SYNC-009 | Sync modes | Each folder is set to keep local (full copy), on demand (placeholders fetched when opened, where the operating system provides placeholder files) or excluded. | T2 | 3 |
| HB-SYNC-010 | Two-way synchronization | Local changes upload as new versions; new remote versions download; renames and moves apply in both directions; local deletions move documents to the project trash. | T2 | 3 |
| HB-SYNC-011 | Conflict preservation | When a file changed both locally and remotely, the remote version stays latest and the local file is kept as a conflict copy next to it and uploaded as a conflict document; nothing is overwritten silently. | T2 | 3 |
| HB-SYNC-012 | Lock while open | Where the operating system reports open file handles, the agent can acquire the document lock while a file is open and release it after the upload that follows closing. | T3 | 3 |
| HB-SYNC-013 | Bandwidth and schedule | Users limit upload and download rates, pause synchronization, set schedules and defer large transfers on metered networks. | T2 | 3 |
| HB-SYNC-014 | Path safety | The agent reports names that are invalid on the local operating system, handles long Windows paths and detects names that differ only in case, without renaming Hub documents. | T2 | 3 |
| HB-SYNC-015 | Sync status and log | Users see per-file state, pending transfers and errors, and retry failed items. | T2 | 3 |
| HB-SYNC-016 | Cache management | Users set a cache size, pin folders for offline use; unpinned, unmodified files are evicted least recently used first. | T2 | 3 |

## WebDAV

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SYNC-017 | WebDAV access | Project folders are exposed over WebDAV for operating-system mounts without the agent. Reads honor Read and Download, PUT adds a version subject to naming validation, and WebDAV locks map to document locks. | T2 | 2 |
