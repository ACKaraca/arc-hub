# Construction records

Construction-phase records at a working level: requests for information, submittals, forms,
photos, schedules, cost tracking, estimating, bid management, procurement, meetings and daily logs. All of them are record types on
the shared workflow engine, so permissions, numbering, notifications and reports behave as for
issues.

Domain objects:

- **Request for information (RFI)**: a question with routing, responses and an official answer.
- **Submittal item** and **submittal package**: specification-based deliverables under review.
- **Form template** and **form**: structured checklists and inspections.
- **Photo**: an image with time, location and links.
- **Schedule** and **activity**: an imported program of work.
- **Budget line**, **commitment** and **change event**: light cost tracking.
- **Estimate**, **bid package** and **purchase order**: working-level estimating, bidding and procurement records.
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

## Estimating

Estimates are working-level records that sit in the project cost breakdown beside the budget. The
server computes totals and comparisons; it holds no price books of its own and exchanges cost
data with accounting systems (HB-SITE-017).

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-021 | Estimates | An estimate has versions, each with lines (cost code, description, quantity, unit, unit cost, markup) grouped in sections, with totals per section and per cost code. One currency applies per project. | T3 | 5 |
| HB-SITE-022 | Estimates from takeoff quantities | Lines are created from quantities of takeoff packages (HB-SITE-018) or from element queries (HB-DATA-010) and keep a link to the source. When the source model changes, the estimate shows the changed quantities and updates them only on request. | T3 | 5 |
| HB-SITE-023 | Unit cost libraries | Organizations keep versioned libraries of unit costs and assemblies (a unit cost made of lines) that import from CSV; a line records the library version it took its cost from. | T3 | 5 |
| HB-SITE-024 | Estimate to budget | Approving an estimate version creates or updates budget lines (HB-SITE-014). Two estimate versions compare line by line, with quantity, unit cost and total differences. | T3 | 5 |

## Bid management

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-025 | Bid packages | A bid package names a trade or scope and holds scope documents, a bid form of line items, invited companies, a due date and the instructions. Addenda are issued to every invited company through transmittals (HB-TRANS-001) and clarifying questions are records answered to all bidders. | T3 | 5 |
| HB-SITE-026 | Bid submission | Invited companies submit form values and attachments through their account or an expiring link. Bids stay sealed, visible to nobody including project members, until the due date; the server records opening, and no bid changes after the deadline. | T3 | 5 |
| HB-SITE-027 | Bid comparison | A comparison sheet lines up bids per form line, with inclusions and exclusions marked so that scopes are leveled, totals and spread, and notes per bidder. It exports as CSV and PDF. | T3 | 5 |
| HB-SITE-028 | Award | Awarding a bid records the decision with its reason, notifies bidders and creates a commitment (HB-SITE-015) with its schedule-of-values lines from the bid form. | T3 | 5 |

## Procurement

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-SITE-029 | Purchase orders | A purchase order has vendor, delivery address and dates, and lines linked to budget or commitment lines. It runs through a workflow (draft, issued, partly received, closed) and is issued to the vendor through a transmittal. The server processes no payments. | T3 | 5 |
| HB-SITE-030 | Receipts and order changes | Receipts are recorded against order lines. A change to an issued order is a new revision linked to a change event (HB-SITE-016); committed amounts follow the latest approved revision. | T3 | 5 |
| HB-SITE-031 | Procurement log | The log lists purchase orders with submittal items (HB-SITE-004), lead times and required-on-site dates, and flags items whose expected delivery falls after the required date. It exports as CSV. | T3 | 5 |
