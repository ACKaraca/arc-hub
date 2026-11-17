# Deployment and operations

Installing, running, scaling, backing up, restoring and upgrading an Arc Hub server. Self-hosting
is the default deployment; the design is described in
[`deployment.md`](../../architecture/deployment.md).

Domain objects:

- **Server role**: all, web, worker or cache, selected when the server binary starts.
- **Storage location**: a filesystem path or S3-compatible bucket holding blobs.
- **Schema version**: the database migration level the server requires.
- **Backup**: a consistent pair of database backup and blob storage state.

## Installation

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-OPS-001 | Single server binary | One executable runs the server in the role all (web, API and worker in one process), web or worker, configured by a file and environment variables. Linux on x86-64 and AArch64 is the production platform; Windows and macOS builds serve evaluation and development. | T1 | 2 |
| HB-OPS-002 | Container images | Release builds include OCI container images and a reference composition with PostgreSQL and filesystem storage. | T1 | 2 |
| HB-OPS-003 | Minimal dependencies | A server needs PostgreSQL and a blob storage location (filesystem or S3-compatible). Email delivery and an identity provider are optional; no other service is required. | T1 | 2 |
| HB-OPS-004 | First-run setup | Initialization creates the schema and prints a one-time setup token; the web setup page, protected by that token, creates the first server administrator and organization. | T1 | 2 |
| HB-OPS-005 | Configuration check | `arc-hub-admin config check` validates configuration, database and storage connectivity, TLS material and identity provider metadata and reports each problem before the server starts. | T1 | 2 |
| HB-OPS-006 | TLS | The server terminates TLS with provided certificates or obtains them automatically through ACME, or runs behind a reverse proxy whose address is configured as trusted for forwarded headers. | T1 | 2 |
| HB-OPS-007 | Email delivery | Outgoing email uses SMTP with TLS through a persistent outbox with retries; templates are localized. | T1 | 2 |
| HB-OPS-008 | Disconnected installations | The server makes no outbound connection unless configured to; update checks are off by default and the documentation ships with the server. | T1 | 2 |

## Upgrades

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-OPS-009 | Database migrations | Migrations are embedded in the binary and applied by `arc-hub-admin migrate`, or at startup when enabled. They are forward-only and follow an expand-then-contract pattern; the server refuses to start against a newer schema than it knows. | T1 | 2 |
| HB-OPS-010 | Rolling upgrades | Within a major version, nodes of the old and the new minor release run side by side during an upgrade. A pre-upgrade check reports blockers such as pending contract migrations. | T1 | 2 |
| HB-OPS-011 | Maintenance mode | Administrators switch the server to read-only maintenance mode: reads continue, writes return 503 with a message shown in clients. | T2 | 2 |

## Backup and integrity

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-OPS-012 | Consistent backup | Because blobs are immutable and content-addressed, a database backup followed by a blob storage copy is consistent. `arc-hub-admin backup` produces a single archive for filesystem installations; for S3-compatible storage, bucket versioning or replication is documented. | T1 | 2 |
| HB-OPS-013 | Restore | Restoring to the same or a new server is followed by a verification that every referenced blob is present; missing blobs are listed with the versions affected. | T1 | 2 |
| HB-OPS-014 | Project restore | A single project is restored from a backup into a new project of the running server without affecting other content. | T2 | 3 |
| HB-OPS-015 | Integrity check and garbage collection | The administration command verifies blob digests fully or by sample. Unreferenced blobs are removed only after a grace period of seven days so in-flight uploads are never lost. | T1 | 2 |
| HB-OPS-016 | Storage migration | An organization's blobs move between storage locations while the server runs: copy, verify, switch, then remove from the source. | T2 | 3 |

## Observability and scale

| ID | Capability | Behavior | Tier | Phase |
|---|---|---|---|---|
| HB-OPS-017 | Health endpoints | Liveness and readiness endpoints report process state and database and storage reachability; a version endpoint reports build and schema versions. | T1 | 2 |
| HB-OPS-018 | Metrics | An OpenMetrics endpoint, protected by a token or network restriction, exposes request latency, queue depth, job durations, storage operations and database pool usage. | T1 | 2 |
| HB-OPS-019 | Logs and traces | Logs are structured JSON with request identifiers and contain no secrets or file content; OpenTelemetry traces export over OTLP when configured. | T1 | 2 |
| HB-OPS-020 | Horizontal scaling | Web nodes are stateless behind a load balancer; sessions and push fan-out go through PostgreSQL, and workers scale independently. | T2 | 3 |
| HB-OPS-021 | High availability | With a highly available PostgreSQL service, redundant storage and two or more web nodes, the server has no single process whose loss interrupts service; documented failure modes describe the behavior when each dependency fails. | T2 | 3 |
| HB-OPS-022 | Site cache | A server in the cache role at a remote office serves downloads and viewables from a local copy of blobs it has fetched, authorizing each request with the main server; uploads pass through. | T3 | 4 |
