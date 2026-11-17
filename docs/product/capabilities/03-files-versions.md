# Files and versions

Folders, documents, immutable versions, transfer, locks, references and deletion. This is the
storage layer of the Documents module and of product data: every file in Arc Hub is a document
with a version history.

Domain objects:

- **Folder**: a node of a project's folder tree with grants and folder settings.
- **Document**: a stable lineage of versions with a name and a folder.
- **Version**: an immutable, numbered state of a document pointing to one blob.
- **Blob**: content addressed by its SHA-256 digest, stored once per storage location.
- **Lock**: an exclusive right to add versions to a document.
- **Reference**: a link from one version to another document, pinned to a version or to latest.
- **Trash entry**: a deleted document or folder awaiting restore or purge.

## Folders

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-001 | Folder tree | Each project has a folder tree of unlimited depth. Names are at most 255 characters, full paths at most 1024, sibling names are unique ignoring case, and characters or trailing dots and spaces that are invalid on any desktop operating system are rejected so every tree can be synchronized. | T1 | 2 |
| HB-FILE-002 | Folder settings | A folder carries a naming standard, property set, default review workflow, bound container state or lifecycle category, allowed file extensions and notification defaults. Subfolders inherit each setting unless they override it, and the effective source of every setting is shown. | T1 | 2 |
| HB-FILE-003 | Bulk folder creation | Users with Manage create folder hierarchies from a template subtree or from a CSV list of paths; existing folders are left unchanged and reported. | T2 | 2 |
| HB-FILE-004 | Move and copy | Folders and documents move or copy within a project. Moves keep document identifiers, so references by identifier survive; effective permissions are recomputed at the destination and the user is shown who gains or loses access before confirming. Copies create new documents whose first version records its origin. | T1 | 2 |
| HB-FILE-005 | Cross-project copy | Users copy folders or documents to another project of the same organization when they hold Upload at the destination; each copy records the source project, document and version. | T2 | 2 |

## Documents and versions

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-006 | Document lineage | A document has a stable identifier, a name and a folder. Uploading a file whose name matches an existing document in the same folder adds a version to that document; version numbers start at 1, increase by one and are never reused. | T1 | 2 |
| HB-FILE-007 | Version immutability | The content and system metadata of a version never change. Content is addressed by SHA-256; identical content is stored once but still produces a new version, and the upload dialog warns when the content equals the current version. | T1 | 2 |
| HB-FILE-008 | Version metadata | Each version records uploader, time, size, digest, comment, the client application and its version, and for Arc native documents the source document revision and journal position. Editable properties are kept separately with their own history (HB-INFO-003). | T1 | 2 |
| HB-FILE-009 | Restore earlier version | Restoring an earlier version creates a new version with the earlier content; no version is removed or renumbered. | T1 | 2 |
| HB-FILE-010 | Version history | A document's history lists every version with comment, author, container or lifecycle state, revision label, review outcomes, transmittals and package memberships, and allows opening, downloading or comparing any two versions. | T1 | 2 |
| HB-FILE-011 | Rename documents | Renaming or moving a document creates no version; the change is recorded in the document history and in the audit trail. | T1 | 2 |
| HB-FILE-012 | Any file type | Documents of any file type are stored and versioned. Viewing depends on derivatives (HB-DATA-001); a per-organization maximum file size applies, 50 GiB by default. | T1 | 2 |
| HB-FILE-013 | Permalinks | Every folder, document, version and record has a stable URL based on its identifier that survives renames and moves. Document links resolve to the latest version the reader may see; version links always open that version. | T1 | 2 |

## Transfer

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-014 | Resumable upload | Uploads use the tus resumable protocol in chunks of 8–64 MiB and can resume for 24 hours after an interruption. The server verifies the digest before committing the version; incomplete uploads are never visible. | T1 | 2 |
| HB-FILE-015 | Bulk upload | Users drop files and folders into the browser; the tree is preserved, each file shows its naming and required-property validation result before commit, and the batch can be cancelled as a whole. | T1 | 2 |
| HB-FILE-016 | Direct storage transfer | With S3-compatible storage, clients transfer content through presigned URLs that expire after 15 minutes and are scoped to one object; the server records the version only after verifying the digest. | T2 | 2 |
| HB-FILE-017 | Download and archives | Users download one version, or several documents as a streamed ZIP that contains only files they hold Download on. Archives larger than a configured size are built by a job and offered as an expiring link. | T1 | 2 |
| HB-FILE-018 | Controlled-copy stamping | A folder can require that PDF downloads by non-editors carry a stamp with the reader's name, date and the document's state, applied at download time without changing the stored version. | T3 | 4 |

## Locks

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-019 | Document lock | A user with Edit, or the uploader of a document with Upload, locks it. While locked, only the holder adds versions or renames it; the lock shows holder, time and client. Check-out (HB-PDM-001) and Arc application editing use the same lock. | T1 | 2 |
| HB-FILE-020 | Lock release and override | The holder releases a lock; project administrators and holders of Manage override it with a reason, and the holder is notified. An optional per-project lock expiry releases locks after a configured idle time. | T1 | 2 |

## References

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-021 | Reference graph | For Arc native documents and formats with external references (DWG and DXF external references, STEP external assembly files), the derivative job extracts references, resolves them to documents of the same project by relative path, and stores them pinned to a version or to latest. Each document shows what it uses and what uses it. | T1 | 2 |
| HB-FILE-022 | Download with references | Downloading a version can include every referenced document at its referenced version, written with the relative paths the references expect. | T2 | 2 |
| HB-FILE-023 | Unresolved references | A report lists references that resolve to no document, per folder, with the referencing version and the expected path. | T2 | 3 |

## Deletion and storage

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-FILE-024 | Trash | Deleted documents and folders move to the project trash for the retention period (30 days by default, 1–3650 days per organization). Restore returns them to their original path, or to a chosen folder if the path is taken; administrators purge items early unless a legal hold applies (HB-ADMIN-011). | T1 | 2 |
| HB-FILE-025 | Version purge | Administrators purge intermediate versions older than a chosen age. Versions that carry a revision label, review outcome, transmittal, document set or package membership are always kept; purges are audited and freed blobs are reclaimed by garbage collection. | T2 | 3 |
| HB-FILE-026 | Storage accounting | Folder and project properties show stored bytes for current versions, earlier versions, derivatives and trash. | T2 | 2 |
| HB-FILE-027 | Share links | Users with Manage, where the organization allows it, create links to one version for viewing or download, with expiry, optional password and optional restriction to signed-in members. Links are revocable and every access is recorded. | T2 | 2 |
