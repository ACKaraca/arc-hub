# Reference workflows

Fourteen end-to-end workflows used as acceptance scenarios. Each lists the steps in Arc Hub's
vocabulary and ends with observable acceptance results; capability IDs show what each step
exercises.

## W1. Set up an organization and a project from a template

1. A server administrator creates the organization with its storage location (HB-ORG-002,
   HB-ORG-004) and configures an OpenID Connect provider and SCIM provisioning (HB-ACCESS-002,
   HB-ACCESS-006).
2. The organization administrator imports companies from CSV (HB-ORG-020) and creates a project
   from a template (HB-ORG-014) with the Documents and Design collaboration modules (HB-ORG-008).
3. The project administrator adds members by company and role (HB-ACCESS-012) and adjusts folder
   grants, previewing who gains access (HB-ACCESS-024).
4. The project administrator configures the naming standard and required properties on the
   work-in-progress folders (HB-INFO-007, HB-INFO-002) and registers a webhook for an external
   system (HB-API-016).

**Acceptance**

- The project contains the template's folders, grants, issue types, review workflows and naming
  standard; editing the template afterwards does not change the project.
- A provisioned user signs in through the identity provider and sees only this project.
- The permission explanation for a member on a restricted folder names the grant that applies.
- Creating a document in the project delivers a signed event to the webhook target.

## W2. Share an information container through ISO 19650 states

1. An author uploads a model to a task team area; the name is validated and fields populate
   properties (HB-INFO-008, HB-INFO-010, HB-INFO-018).
2. The author requests the shared state; a gate review of the check-review-approve template
   starts (HB-INFO-014, HB-REVIEW-004, HB-REVIEW-005).
3. Reviewers comment and approve per item (HB-REVIEW-006, HB-REVIEW-007).
4. Final actions set the shared state, suitability and preliminary revision code (HB-REVIEW-009,
   HB-INFO-015, HB-INFO-016).
5. Later an authorization review publishes the container; the earlier published revision is
   superseded (HB-INFO-017).

**Acceptance**

- A non-compliant name is rejected with the failing field named.
- Before approval, members of other task teams cannot list the container; after the shared state,
  they can view and download it according to the state caps (HB-ACCESS-020).
- The version shows its state, suitability, revision code and review status; the container
  register lists it (HB-INFO-019).
- The review record lists each decision with participant, time and version digest
  (HB-REVIEW-015).

## W3. Review drawings with markups and issue them by transmittal

1. An Arc application publishes a sheet set; sheets enter the register with number, title and
   revision (HB-VIEW-018, HB-VIEW-016).
2. A document controller starts a review of the sheets (HB-REVIEW-005).
3. Reviewers mark up sheets in the browser and convert one markup into an issue (HB-MARKUP-001,
   HB-MARKUP-009, HB-MARKUP-012).
4. On approval, the review markups are published to the versions (HB-MARKUP-006).
5. The controller captures a document set and sends a transmittal to the contractor
   (HB-VIEW-019, HB-TRANS-001, HB-TRANS-008).

**Acceptance**

- Sheets stack as new revisions when republished with the same numbers.
- The issue opens at the markup's location with its snapshot.
- The contractor's members can download exactly the transmitted versions and nothing else in
  those folders (HB-TRANS-004).
- The sender sees the first opening and each download per recipient (HB-TRANS-005); the
  transmittal cannot be edited afterwards.

## W4. Resolve an issue between the browser, BCF tools and an Arc application

1. A reviewer creates an issue pinned to an element in the browser viewer (HB-ISSUE-004).
2. A consultant's BCF-capable tool synchronizes the issue through the BCF API and adds a comment
   with a viewpoint (HB-ISSUE-021, HB-ISSUE-006).
3. The assignee opens the issue in ArchBuilder; the viewpoint and selection are restored
   (HB-ISSUE-022).
4. The assignee changes the model, saves a new version to the Hub and moves the issue to resolved
   with a reference to the new version (HB-SYNC-003, HB-ISSUE-005).
5. The author closes the issue with a root cause (HB-ISSUE-003, HB-ISSUE-012).

**Acceptance**

- The issue, comment and viewpoint are identical in the browser, in the BCF tool and in the Arc
  application, with the same GUIDs.
- Closing without a root cause is refused when the type requires one.
- Exporting the issue as BCF 3.0 and importing it into an empty project reproduces it
  (HB-ISSUE-019, HB-ISSUE-020).

## W5. Work on Hub documents from an Arc application

1. A designer opens a DraftBuilder drawing from the Hub; referenced drawings download at their
   referenced versions (HB-SYNC-002, HB-FILE-021).
2. Starting to edit takes the lock (HB-SYNC-004, HB-FILE-019).
3. The designer goes offline, continues working, reconnects and saves a new version with a
   comment (HB-SYNC-003).
4. A colleague who edited a copy offline without the lock saves too (HB-SYNC-004).
5. A newer version of a referenced drawing appears; the designer updates the reference
   (HB-SYNC-006).

**Acceptance**

- Other users see the lock holder while the drawing is locked.
- The colleague's work is stored as a conflict document linked to the original, and a comparison
  is offered; no version is lost.
- The upload resumes after a network interruption without restarting.
- The new version records the updated reference.

## W6. Share and consume design packages between teams

1. The structural team defines a package from its model with an element filter for structural
   categories and shares version 1 (HB-PKG-003, HB-PKG-004).
2. The architectural team consumes version 1; ArchBuilder loads it as a linked model
   (HB-PKG-006).
3. The structural team shares version 2; the architectural team is notified and reviews the
   differences (HB-PKG-007, HB-PKG-008).
4. The architectural team updates to version 2, then reverts to version 1 (HB-PKG-007).

**Acceptance**

- Version 1's content never changes after sharing.
- The shared timeline shows share, consume, update and revert events with authors and times
  (HB-PKG-005).
- The difference list names added, removed and changed elements by identifier.
- Members outside both teams cannot open the structural team area (HB-PKG-002).

## W7. From automatic clash run to closed issue

1. A coordinator creates a model set in latest mode with architectural, structural and services
   groups and a clearance test (HB-COORD-001, HB-COORD-004).
2. A new services model version triggers an incremental clash run in the job worker
   (HB-COORD-005, HB-JOB-003).
3. The coordinator reviews grouped clashes in the browser and creates issues from two groups
   (HB-COORD-007, HB-COORD-008, HB-COORD-009).
4. The services engineer changes the model and saves a new version; the next run no longer
   detects the clashes (HB-COORD-006).
5. The engineer accepts the resolution suggestion and the coordinator closes the issues.

**Acceptance**

- Clashes keep their identity across runs and show new, active, resolved and reappeared states.
- Only pairs involving changed elements are recomputed in the second run.
- The issues show the live clash status and receive a resolution suggestion after the clean run.
- The clash report shows the drop in active clashes between the two snapshots (HB-COORD-010).

## W8. Edit one model with several people

1. A project administrator enables multi-user mode on an ArchBuilder model (HB-SESSION-001).
2. Two architects join; both see each other in the session (HB-SESSION-002, HB-SESSION-006).
3. Architect A borrows and edits walls on level 1; architect B's attempt to edit one of them is
   refused and B sends a borrow request (HB-SESSION-004, HB-SESSION-005).
4. Both synchronize; each receives the other's change sets (HB-SESSION-003, HB-SESSION-007).
5. B deletes a door offline that A edited meanwhile; on reconnect B's change becomes a conflict
   record, resolved by B in the application (HB-SESSION-009, HB-SESSION-008).
6. The administrator publishes the session state as a version for review (HB-SESSION-011).

**Acceptance**

- No change set modifying an element borrowed by another user is accepted.
- Borrow responses on a local network meet the 200 ms target.
- The conflict record holds both states until resolution; nothing is discarded.
- The published version opens in any Arc application and in the browser viewer.

## W9. Release a part with lifecycle and revision

1. A designer checks out a SolidBuilder part, edits it and checks it in with a comment
   (HB-PDM-001, HB-PDM-002).
2. Category rules assign the part's lifecycle, revision scheme and number (HB-PDM-020,
   HB-PDM-022).
3. The designer requests the transition to released; conditions are checked (HB-PDM-014).
4. Transition actions assign revision A, lock the file, write the revision into the file
   properties and queue PDF and STEP outputs (HB-PDM-015, HB-PDM-019, HB-PDM-026).
5. The item is created from the part and its structure extracted (HB-ITEM-002, HB-ITEM-009).

**Acceptance**

- The transition is refused while a referenced child is not released, listing that child.
- After release, designers cannot check out the part (state cap) and the document shows
  revision A with its version number.
- The PDF and STEP outputs exist as derivatives of the released version.
- The item revision is frozen with the released document version.

## W10. Engineering change with where-used and enterprise publication

1. An engineer raises a change request for a released part and promotes it to a change order
   (HB-ECO-001, HB-ECO-002).
2. The engineer runs impact analysis and adds the affected assemblies (HB-ITEM-016, HB-ECO-004).
3. Approvers compare before and after, including the BOM comparison, and approve with signatures
   (HB-ECO-005, HB-ECO-006).
4. Closing the change order releases all affected records at revision B (HB-ECO-007).
5. The release publication sends the changed items and structures to the enterprise system,
   which acknowledges them (HB-ECO-012, HB-ECO-014).

**Acceptance**

- Affected records cannot be added to another change order while this one is active.
- If one affected record fails a condition at closing, no record changes and the failure is
  listed.
- Each item revision shows the publication status returned by the enterprise system.
- The where-used list shows every top-level assembly using the part.

## W11. Copy a design for a variant

1. An engineer starts copy design on an assembly drawing (HB-ITEM-017).
2. The engineer sets copy for the assembly and two parts, reuse for standard parts, replace for a
   bracket and a numbering-scheme naming rule.
3. The preview shows new names and the destination folder; the engineer runs the job.
4. The engineer opens the new assembly in SolidBuilder.

**Acceptance**

- New documents exist with generated numbers, at the initial lifecycle state, linked to their
  sources.
- References in the new assembly and drawing point to the new parts, the reused standard parts and
  the replacement bracket.
- The source assembly and its documents are unchanged.

## W12. Shot production loop with client review

1. A producer imports a shot list from an edit decision list and applies task templates
   (HB-PROD-006, HB-PROD-008).
2. The coordinator auto-schedules tasks against artists' calendars and reviews the proposal
   (HB-PLAN-003, HB-PLAN-002).
3. An artist opens a task in MeshBuilder, publishes outputs and a submission (HB-PROD-019,
   HB-PROD-017, HB-MEDIA-001).
4. The supervisor reviews a daily playlist in the browser player, annotates frames and sets
   statuses (HB-MEDIA-009, HB-MEDIA-006, HB-MEDIA-010).
5. Approved shots go to a client review link; the client approves (HB-MEDIA-012, HB-MEDIA-011).
6. The downstream lighting task becomes ready (HB-PROD-011).

**Acceptance**

- Shots carry cut in and out from the edit decision list.
- The schedule proposal respects dependencies and leave and is identical when recomputed.
- Notes with annotations reach the artist and appear on the task.
- The client accesses only the playlist through an expiring link; proxies carry the watermark.
- The downstream task's status changes without manual action.

## W13. Event-driven automation with a headless Arc job

1. An administrator defines a job that runs DraftBuilder's command-line tool to plot drawings to
   PDF with a parameter schema (HB-JOB-006, HB-JOB-004).
2. A trigger starts the job when a drawing enters the shared state (HB-JOB-007).
3. The run executes in a sandbox with its job token and uploads PDFs next to the drawings
   (HB-JOB-005).
4. An external system subscribed to the job-completed event fetches the PDFs through the API
   (HB-API-016, HB-API-008).

**Acceptance**

- The job's parameter form is generated from the schema and rejects invalid values.
- The run cannot reach any network address except the Hub.
- PDFs are new versions whose origin is the job run, visible in the audit trail.
- The webhook delivery is signed and retried if the receiver is down.

## W14. Install, back up, restore and upgrade a server

1. An administrator installs the server with PostgreSQL and filesystem storage and completes
   first-run setup (HB-OPS-001, HB-OPS-004, HB-OPS-005).
2. Users work for a period; the administrator runs a backup (HB-OPS-012).
3. The administrator restores the backup to a new machine and verifies it (HB-OPS-013,
   HB-OPS-015).
4. The administrator upgrades the original server to the next minor release with two nodes in a
   rolling upgrade (HB-OPS-009, HB-OPS-010).
5. The administrator verifies audit chains and exports one organization (HB-ADMIN-002,
   HB-ADMIN-012).

**Acceptance**

- The restored server serves every version with matching digests, and verification reports no
  missing blobs.
- During the rolling upgrade, users continue to browse and upload without errors.
- Audit chain verification passes on the restored and upgraded servers.
- The organization export imports into a fresh server with identifiers and histories preserved.
