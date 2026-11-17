# Coordination

Model sets, automatic clash runs and automatic model checking on the server. Detection itself is
performed by the headless Arc Review application in the job worker; the Hub keeps sets, tests,
results, statuses and their history.

Domain objects:

- **Model set**: a named set of model documents with transforms and discipline groups.
- **Model set snapshot**: the exact versions of a model set at one time.
- **Clash test**: groups, detection type, tolerance and rules.
- **Clash**: a detected conflict keyed by test and element pair, with status over time.
- **Clash group**: an aggregation of clashes for review and assignment.
- **Check rule set** and **finding**: rule-based model checks and their results.

## Model sets

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-COORD-001 | Model sets | A model set names documents or whole folders of a project, each with a discipline group and transform, in latest mode (new versions join automatically) or frozen mode (pinned versions). Every change creates a snapshot, and earlier snapshots remain viewable. | T2 | 3 |
| HB-COORD-002 | Alignment | Transforms come from each document's shared coordinates (Arc georeference, IFC map conversion) and units are normalized; users override the transform of a member. | T2 | 3 |
| HB-COORD-003 | Model set comparison over time | Two snapshots of a model set are compared element by element across all members. | T3 | 4 |

## Clash detection

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-COORD-004 | Clash tests | A test runs between groups A and B or within one group, as hard, clearance (with distance) or duplicate detection, with tolerance and exclusion rules based on property filters, same element and same system. Tests can be saved as templates. | T2 | 3 |
| HB-COORD-005 | Automatic clash runs | When a model set in latest mode receives a new version, the affected tests run in the job worker. Runs are incremental: only pairs involving changed elements are recomputed, and results are stored per snapshot. | T2 | 3 |
| HB-COORD-006 | Clash identity across runs | A clash is identified by test and element identifier pair. Across runs it is new, active, resolved (no longer detected) or reappeared; manual statuses (approved, ignored) persist across runs. | T2 | 3 |
| HB-COORD-007 | Clash grouping | Clashes group automatically by element, by level or zone, by proximity within a distance threshold, or by system; users regroup manually, and a group's status aggregates its clashes. | T2 | 3 |
| HB-COORD-008 | Browser clash review | In the browser, users open a model set with its clash list, isolate each pair or group, review viewpoints, assign and change status. | T2 | 3 |
| HB-COORD-009 | Clash to issue | An issue created from a clash or group carries viewpoint and element references and an assignee proposed from discipline rules. The issue shows the live clash status; when the clash resolves, the assignee is asked to resolve the issue. | T2 | 3 |
| HB-COORD-010 | Clash reports | Reports show counts per test, group and status over time and export as CSV, BCF and PDF; clash results also export in a documented JSON format. | T2 | 3 |
| HB-COORD-011 | Run limits | Administrators set maximum pairs per run and a job time limit; a run that hits a limit stores partial results and is labeled partial. | T2 | 3 |

## Model checking

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-COORD-012 | Rule-based model checking | Rule sets made of IDS files and property rules written in the query language (required properties, value ranges, naming, spatial containment) run automatically on new versions. Findings reference elements and can become issues. | T2 | 3 |
| HB-COORD-013 | Coordination dashboard | Per model set, a dashboard shows the age of each discipline's latest version, open clashes per test and open findings per rule set. | T2 | 3 |
| HB-COORD-014 | Arc Review synchronization | Arc Review opens model sets, tests and results from the Hub, edits tests and pushes status changes and issues back. | T2 | 3 |
