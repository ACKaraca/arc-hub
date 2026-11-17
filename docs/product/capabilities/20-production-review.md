# Production tracking: submissions, playlists and review

Review of work in progress and final outputs: submissions with media, browser playback,
annotations, notes, playlists, live review rooms and client review.

Domain objects:

- **Submission**: a reviewable output of a task with media, status and links to published outputs.
- **Proxy media**: browser-playable renditions, thumbnails and filmstrips produced by jobs.
- **Note**: a threaded comment on an entity or submission, with addressees and frame reference.
- **Annotation**: a drawing on a frame, stored with its note.
- **Playlist**: an ordered list of submissions for review.
- **Review room**: a live, synchronized review session.
- **Client review link**: an expiring external review of a playlist.

## Submissions and media

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MEDIA-001 | Submissions | A submission links a task and entity, published outputs and media (movie, image sequence, still image or 3D viewable), with frame range, frame rate, status, author and description. | T2 | 4 |
| HB-MEDIA-002 | Media processing | Uploaded source media is processed by jobs into browser proxies, thumbnails and filmstrips. Image sequences are converted with the project's OpenColorIO configuration and chosen view transform. | T2 | 4 |
| HB-MEDIA-003 | Browser player | The player is frame-accurate, with frame stepping, loop ranges, timecode and frame display, A/B wipe and side-by-side comparison, playback speed and audio, all operable by keyboard. | T2 | 4 |
| HB-MEDIA-004 | Previous submission comparison | The player compares a submission with earlier submissions of the same task in one action. | T2 | 4 |
| HB-MEDIA-005 | 3D submissions | Submissions can carry 3D viewables reviewed in the embedded viewer (HB-VIEW-001) with the same notes and statuses. | T3 | 4 |

## Notes and annotations

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MEDIA-006 | Annotations | Reviewers draw and type on frames; annotations are saved with frame number as images attached to a note. | T2 | 4 |
| HB-MEDIA-007 | Notes | Notes on any entity or submission have threads, To and CC addressees (users or groups), attachments, frame or time reference and open or closed status; assignees of the linked task are notified. | T2 | 4 |
| HB-MEDIA-008 | Notes exchange | Notes of a playlist export as PDF and CSV with thumbnails and frame references; notes import from CSV matched by submission. | T2 | 4 |

## Playlists and review

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-MEDIA-009 | Playlists | Playlists are ordered lists of submissions built by hand or from a saved filter, which keeps updating until the playlist is locked. | T2 | 4 |
| HB-MEDIA-010 | Review session | Reviewers step through a playlist, write notes and set status per item inline; the session records each item's outcome. | T2 | 4 |
| HB-MEDIA-011 | Approval statuses | Internal and client approval are separate statuses on a submission; approvals update task status through propagation rules (HB-PROD-011). | T2 | 4 |
| HB-MEDIA-012 | Client review links | An expiring link with optional password opens a playlist for external reviewers without an account; they comment and approve per item, proxies are watermarked with the reviewer's email, and every access is recorded. | T2 | 4 |
| HB-MEDIA-013 | Live review rooms | In a review room the presenter controls playback and participants see the same frame within a target of 250 ms, with shared annotations and pointer. | T3 | 5 |
| HB-MEDIA-014 | Desktop players | Desktop review players open playlists through the documented API and write notes and statuses back. | T3 | 4 |
| HB-MEDIA-015 | Editorial cuts | Cuts import from OpenTimelineIO or edit decision lists as cut versions; differences list added and removed shots and changed in and out points, and approved cut changes update shot fields. | T3 | 4 |
| HB-MEDIA-016 | Deliveries | Approved submissions and outputs are delivered to recipients through transmittals (HB-TRANS-001) after checks of resolution, frame rate and naming. | T3 | 4 |
