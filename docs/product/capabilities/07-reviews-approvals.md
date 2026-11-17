# Reviews and approvals

Configurable review and approval workflows over document versions, sets and packages, with
recorded decisions that can change container states and move files. The same workflow engine
drives change orders and submittals.

Domain objects:

- **Workflow template**: initiators, ordered steps, participants, decision options and final actions.
- **Review**: one run of a workflow over a list of review items.
- **Review item**: one document version under review, with a decision per approval step.
- **Decision**: a participant's outcome on items, with comment, time and optional signature.

## Workflow templates

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-REVIEW-001 | Workflow templates | Administrators define a template with name, permitted initiators, ordered steps, participants per step (users, roles, groups, companies), step kind (review collects comments; approval collects decisions), due offset in working days, decision options each mapped to approve, reject or neutral, and final actions. | T1 | 2 |
| HB-REVIEW-002 | Step completion rules | Each step completes when all participants, any one participant or a minimum number has decided; for role, group and company participants the template states whether any member or each member must act. On rejection the review returns to the initiator, returns to the previous step or ends, as configured. | T1 | 2 |
| HB-REVIEW-003 | Parallel branches | Steps normally run in sequence with participants of a step acting in parallel. A template can also define parallel branches of steps that join before the next step. | T2 | 3 |
| HB-REVIEW-004 | Gate rules | A folder or container-state transition can require an approved review of a given template, for example entering the Shared state (HB-INFO-014). The transition is refused until such a review exists for the version. | T1 | 2 |

## Running reviews

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-REVIEW-005 | Start a review | An initiator selects document versions, a document set, a document package or sheets they hold Download on, chooses a template, adds optional participants the template allows and sets the due date; the review receives a project number. | T1 | 2 |
| HB-REVIEW-006 | Per-item decisions | Approval participants decide per item, so one review can approve some versions and reject others; the step outcome summarizes item decisions. | T1 | 2 |
| HB-REVIEW-007 | Review workspace | Participants open each item in the viewer with the review's markup set, comment per item, raise issues, and see decisions and comments of earlier steps. | T1 | 2 |
| HB-REVIEW-008 | Review status on versions | The final outcome of each item is written to its version as the review status (for example approved, approved with comments, rejected) and shown in folder lists. A new version of the document starts without a review status. | T1 | 2 |
| HB-REVIEW-009 | Final actions | On completion, per outcome, the server copies or moves versions to a target folder, sets container state and suitability, sets properties, drafts a transmittal or emits an event for automation. Actions run under the workflow's service identity; failed actions are shown and can be retried. | T1 | 2 |
| HB-REVIEW-010 | Delegation and reassignment | Participants delegate their step to another eligible member; administrators reassign; users set out-of-office delegation for a date range in their profile. | T2 | 2 |
| HB-REVIEW-011 | Reminders and escalation | Participants receive reminders before the due date and when overdue; after a configured number of overdue days the step owner or project administrator is notified. | T2 | 2 |
| HB-REVIEW-012 | Cancel and restart | The initiator or an administrator cancels a review with a reason; restarting creates a new review linked to the cancelled one, optionally with newer versions. | T1 | 2 |
| HB-REVIEW-013 | External reviewers | Guests can be participants; they receive Download on the review items for the duration of the review only, and the temporary grant is removed when their step ends. | T2 | 2 |
| HB-REVIEW-014 | My reviews | Each user sees reviews waiting for their action, reviews they started and overdue reviews across projects. | T1 | 2 |

## Records and signatures

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-REVIEW-015 | Review record | Every decision is stored with participant, time, outcome, comment and the digests of the decided versions; a review report PDF lists items, steps, decisions and comments. | T1 | 2 |
| HB-REVIEW-016 | Electronic signatures | A step can require signature: the participant re-authenticates (fresh sign-in or WebAuthn) and the decision stores signer, time, stated meaning and version digests in a signature manifest. An optional visible stamp is added to the review report. | T2 | 3 |
| HB-REVIEW-017 | Comment-response register | Review comments receive a response from the document author and a closure state; the register exports as CSV or PDF per review. | T2 | 3 |
