# Design packages

Structured exchange of design data between teams of one project and between organizations:
teams curate packages, share sealed package versions on a shared timeline, and other teams
consume them at pinned versions.

Domain objects:

- **Team**: a named group of project members by discipline or company, with a team area.
- **Design package**: a team's curated definition of documents and element filters.
- **Package version**: a sealed, immutable result of a package at the moment of sharing.
- **Consumption**: a team's adoption of one package version as reference content.
- **Timeline event**: a share, consume, update or revert recorded on the shared timeline.
- **Cross-organization share**: a folder link between projects of different organizations.

## Teams

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PKG-001 | Teams | Project administrators create teams with name, discipline, company, members and team administrators; a member can belong to several teams. | T2 | 3 |
| HB-PKG-002 | Team areas | Each team has a team area folder where only its members hold Edit; other project members hold none or Read, as the project setting says. | T2 | 3 |

## Packages

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PKG-003 | Package definition | A team defines a package with name, description, recipient teams and contents: documents at latest or specific versions and, for Arc model documents, element filters (categories, levels, views and sheets) that a job evaluates into a filtered derivative. | T2 | 3 |
| HB-PKG-004 | Share a package version | Sharing seals a numbered package version: the exact document versions, the filter results and a note. Recipients are notified; the sealed content never changes. | T2 | 3 |
| HB-PKG-005 | Shared timeline | The project timeline lists every package version shared and every consumption, update and revert, with team, author and time, filterable by team; any package version on it opens in the viewer. | T2 | 3 |
| HB-PKG-006 | Consume a package | A recipient team consumes a package version: its content joins the team's reference set, which Arc applications load as linked models at the pinned versions. | T2 | 3 |
| HB-PKG-007 | Update and revert | When a newer package version is shared, consumers are notified and choose to update or stay; a consumer can revert to any earlier consumed version. Every choice is a timeline event. | T2 | 3 |
| HB-PKG-008 | Package differences | The differences between two package versions are listed per document and per element (HB-VIEW-023) and shown in color in the viewer. | T2 | 3 |
| HB-PKG-009 | Approval before sharing | A team can require an approved review (HB-REVIEW-001) of the package contents before a package version is sealed. | T3 | 3 |
| HB-PKG-010 | Coordination from consumption | Each team's own models plus its consumed packages can form a model set (HB-COORD-001) that updates automatically when consumption changes. | T3 | 3 |

## Cross-organization sharing

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-PKG-011 | Cross-organization shares | A project shares a folder with a project of another organization on the same server, one-way (the recipient receives a read-only mirror of selected versions, updated on each new version or on explicit publish) or two-way (both sides upload into a shared exchange folder). The owner sets the maximum level and can revoke; the recipient administers access inside that maximum. | T2 | 3 |
| HB-PKG-012 | Share records | Both organizations record every share event (created, version mirrored, revoked) in their activity and audit trails. | T2 | 3 |
| HB-PKG-013 | Federation between servers | A project shares a folder or package with a project on another Arc Hub server through a signed server-to-server API; content is copied with digests verified. Revocation stops further mirroring; copies already received remain with the recipient. | T3 | 5 |
