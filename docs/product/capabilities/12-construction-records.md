# Construction records

Construction-phase records at a deliberately light level: requests for information, submittals,
forms, photos, schedules, cost tracking, meetings and daily logs. All of them are record types on
the shared workflow engine, so permissions, numbering, notifications and reports behave as for
issues.

Domain objects:

- **Request for information (RFI)**: a question with routing, responses and an official answer.
- **Submittal item** and **submittal package**: specification-based deliverables under review.
- **Form template** and **form**: structured checklists and inspections.
- **Photo**: an image with time, location and links.
- **Schedule** and **activity**: an imported program of work.
- **Budget line**, **commitment** and **change event**: light cost tracking.
- **Meeting** and **daily log**.

## Requests for information

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-001 | RFI records | An RFI has number, subject, question, discipline, references to sheets with pins, elements and documents, raising company, responsible party, due date, priority and cost and schedule impact flags, with the default workflow draft, open, answered, closed. One response is marked as the official answer. | T2 | 4 |
| HB-SITE-002 | RFI routing | The coordinator distributes an RFI to responders, collects responses and forwards the official answer; the server tracks which party holds the next action and the days each party held it. | T2 | 4 |
| HB-SITE-003 | RFI log | The RFI log filters and exports as CSV and as a PDF report with references and answers. | T2 | 4 |

## Submittals

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-004 | Submittal register | Submittal items carry specification section, title, type (for example product data, shop drawing, sample), required date and responsible company; the register imports from CSV. | T2 | 4 |
| HB-SITE-005 | Submittal review | Submittals run through a review workflow (HB-REVIEW-001) with configurable response codes; resubmissions are new revisions of the item that keep the full history. | T2 | 4 |
| HB-SITE-006 | Submittal packages | Submittal items are grouped into packages and issued and answered through transmittals (HB-TRANS-001). | T2 | 4 |

## Forms and photos

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-007 | Form templates | Administrators build versioned form templates with sections and fields (text, number, yes/no/not applicable, choice, date, signature, photo, location), conditional visibility and scoring. | T2 | 4 |
| HB-SITE-008 | Form completion | Members fill forms in the browser, attach photos and sign; a submitted form becomes immutable and is stored as data and as a PDF. A failed check item can create a linked issue. | T2 | 4 |
| HB-SITE-009 | Recurring forms | A form can be scheduled daily or weekly and assigned to a role; missed instances are reported. | T3 | 4 |
| HB-SITE-010 | Photo log | Photos upload with capture time and location from their metadata, are organized in albums with tags, link to locations, issues and RFIs, appear on a map when geolocated, and 360-degree images open in an equirectangular viewer. | T2 | 4 |

## Schedules

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-011 | Schedule import | Programs import from Microsoft Project XML, Primavera P6 XML and CSV; each import is kept as a schedule version, and one version can be marked as baseline. | T2 | 4 |
| HB-SITE-012 | Schedule views | A Gantt view and a look-ahead of two to six weeks filter activities; activities link to issues, RFIs and model elements, and these links are available to Arc Review for 4D sequencing. | T2 | 4 |
| HB-SITE-013 | Schedule export | Milestones export as iCalendar and activities as CSV. | T3 | 4 |

## Cost

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-014 | Budget | A project cost breakdown holds budget lines with original budget, approved changes, committed and actual amounts; the revised budget and forecast are computed. One currency applies per project. | T3 | 5 |
| HB-SITE-015 | Commitments | Contracts and purchase commitments hold schedule-of-values lines linked to budget lines. | T3 | 5 |
| HB-SITE-016 | Change events | Potential changes carry estimated cost and links to RFIs and issues; approving a change event updates the budget lines it affects. | T3 | 5 |
| HB-SITE-017 | Cost exchange | Budget, commitment and actual amounts import from and export to CSV for accounting systems. The server processes no payments. | T3 | 5 |
| HB-SITE-018 | Quantity references | Quantity takeoff packages produced by Arc Review are stored as documents and their totals linked to budget lines; the Hub does not compute quantities. | T3 | 5 |

## Meetings and daily logs

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-019 | Meetings | Meetings hold agenda items, attendees, minutes and action items created as issues of a chosen type; open items carry over to the next meeting of the series, and minutes export as PDF. | T3 | 4 |
| HB-SITE-020 | Daily logs | A daily log records weather as entered, workforce per company, equipment, notes and photos, and becomes read-only after sign-off. | T3 | 5 |
