# Transmittals and document packages

Formal, recorded issue of document versions to recipients inside and outside the project, and the
assembly of document packages used as units for submission, review and transmittal.

Domain objects:

- **Transmittal**: an immutable record of versions sent to recipients with a purpose.
- **Transmittal template**: numbering pattern, purpose codes, default message and cover sheet fields.
- **Receipt** and **response**: what each recipient opened, downloaded and answered.
- **Document package**: a named, versioned collection of document versions for submission.

## Transmittals

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-TRANS-001 | Create a transmittal | A user selects versions (specific, or latest resolved at send time), recipients (members, companies, external email addresses), subject, message, purpose code and whether a response is required by a date. Sending creates an immutable record. | T1 | 2 |
| HB-TRANS-002 | Numbering | Transmittals are numbered per project from a pattern such as `{project}-TRN-{seq:4}`; numbers are unique and never reused. | T1 | 2 |
| HB-TRANS-003 | Transmittal templates | Administrators define templates with numbering pattern, purpose codes, default message and the fields shown on the cover sheet. | T2 | 2 |
| HB-TRANS-004 | Delivery | Recipients receive an email with a link. Members gain Download on exactly the transmitted versions, not on their folders. External recipients get an expiring link that requires a one-time code sent to their email address. | T1 | 2 |
| HB-TRANS-005 | Receipts | The transmittal records each recipient's first opening and every download with time; the sender sees status per recipient. | T1 | 2 |
| HB-TRANS-006 | Responses | Recipients respond per transmittal or per document with a response code, comment and optional files, which are stored as response attachments linked to the transmittal. | T2 | 2 |
| HB-TRANS-007 | Supersede | A sent transmittal cannot be edited; a correction is a new transmittal that references the one it supersedes. | T1 | 2 |
| HB-TRANS-008 | Cover sheet | Sending generates a PDF cover sheet listing documents (name, title, revision, container state, suitability), recipients and purpose, stored with the transmittal. | T1 | 2 |
| HB-TRANS-009 | Transmittal register | The register lists, filters and exports transmittals; each document shows which of its versions were sent to whom and when. | T1 | 2 |
| HB-TRANS-010 | Incoming transmittals | A document controller registers documents received from outside the server as an incoming transmittal with sender company and date, filing each document into a folder. | T2 | 3 |

## Document packages

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-TRANS-011 | Document packages | A user assembles a named package of document versions with purpose and due date. A draft package is editable; submitting locks its contents. Packages are units for reviews (HB-REVIEW-005) and transmittals. | T1 | 2 |
| HB-TRANS-012 | Package versions | Packages move from draft to submitted to superseded or closed; revising a submitted package creates a new package version and supersedes the earlier one, keeping both. | T2 | 2 |
| HB-TRANS-013 | Package checks | Before submission, the server checks every version for naming compliance, required properties and allowed container states and lists failures; administrators decide whether failures block submission. | T2 | 2 |
| HB-TRANS-014 | Package download | A package downloads as a ZIP with a manifest in CSV and JSON listing document identifiers, version numbers, revision labels and digests. | T1 | 2 |
