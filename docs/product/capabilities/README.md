# Capabilities

The capability specification of Arc Hub: one file per functional area, grouped by the five product
modules. Each row states observable behavior precise enough to implement and test.

## Legend

| Column | Meaning |
|---|---|
| ID | `HB-<AREA>-<NNN>`. `HB` is the Arc Hub prefix, `AREA` the area token of the file, `NNN` a number unique in the area. Identifiers are permanent; a withdrawn capability keeps its number and is marked withdrawn. |
| Capability | Short descriptive name. |
| Behavior | What the product does: inputs, result, what updates automatically, limits. |
| Tier | **T1 Core**: required for first production use. **T2 Professional**: required for parity with established professional tools. **T3 Advanced**: specialized or differentiating. |
| Phase | The suite phase in which the capability is planned (0–5); see the [roadmap](../../roadmap.md). |

Status is not tracked in these tables. Work is tracked in GitHub issues: each issue names the
capability IDs it implements in its scope section and carries the `area: <AREA>` label, and an
issue is done only when the behavior of those rows is met and verified.

## Areas

| File | Area | Module | Rows |
|---|---|---|---|
| [01-organizations-projects.md](01-organizations-projects.md) | ORG | Documents | 26 |
| [02-identity-access.md](02-identity-access.md) | ACCESS | Documents | 27 |
| [03-files-versions.md](03-files-versions.md) | FILE | Documents | 31 |
| [04-viewing-comparison.md](04-viewing-comparison.md) | VIEW | Documents | 28 |
| [05-markups.md](05-markups.md) | MARKUP | Documents | 16 |
| [06-issues.md](06-issues.md) | ISSUE | Documents | 22 |
| [07-reviews-approvals.md](07-reviews-approvals.md) | REVIEW | Documents | 17 |
| [08-transmittals-packages.md](08-transmittals-packages.md) | TRANS | Documents | 14 |
| [09-information-management.md](09-information-management.md) | INFO | Documents | 22 |
| [10-search.md](10-search.md) | SEARCH | Documents | 11 |
| [11-desktop-sync.md](11-desktop-sync.md) | SYNC | Documents | 17 |
| [12-construction-records.md](12-construction-records.md) | SITE | Documents | 31 |
| [13-design-packages.md](13-design-packages.md) | PKG | Design collaboration | 13 |
| [14-multi-user-editing.md](14-multi-user-editing.md) | SESSION | Design collaboration | 17 |
| [15-coordination.md](15-coordination.md) | COORD | Design collaboration | 14 |
| [16-pdm-files.md](16-pdm-files.md) | PDM | Product data | 27 |
| [17-items-structure.md](17-items-structure.md) | ITEM | Product data | 22 |
| [18-change-orders.md](18-change-orders.md) | ECO | Product data | 15 |
| [19-production-entities.md](19-production-entities.md) | PROD | Production tracking | 21 |
| [20-production-review.md](20-production-review.md) | MEDIA | Production tracking | 16 |
| [21-production-planning.md](21-production-planning.md) | PLAN | Production tracking | 13 |
| [22-api-webhooks.md](22-api-webhooks.md) | API | Platform | 23 |
| [23-automation-jobs.md](23-automation-jobs.md) | JOB | Platform | 22 |
| [24-model-data.md](24-model-data.md) | DATA | Platform | 19 |
| [25-administration.md](25-administration.md) | ADMIN | Platform | 18 |
| [26-operations.md](26-operations.md) | OPS | Platform | 30 |
| [27-integrations.md](27-integrations.md) | INTEG | Platform | 21 |
| [28-assistant.md](28-assistant.md) | ASSIST | Platform | 10 |
| [29-mobile-clients.md](29-mobile-clients.md) | MOBILE | Platform | 17 |
| [30-application-integrations.md](30-application-integrations.md) | PIPE | Platform | 14 |
| **Total** | | | **594** |

Organizations, identity and access serve every module; they are listed under Documents because
that module is delivered first.

Reference workflows that combine these capabilities into acceptance scenarios are in
[`workflows.md`](../workflows.md); behaviors that still need a decision are in
[`open-questions.md`](../open-questions.md).
